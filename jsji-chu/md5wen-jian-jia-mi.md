```js
import SparkMD5 from 'spark-md5';

/***
 * params: file: input上传的微文件
 * return Promise<加密后的字符串>;
 * */

export default function getFileMd5(file: any):  Promise<string> {
  let blobSlice = File.prototype.slice || (File.prototype as any).mozSlice || (File.prototype as any).webkitSlice,
    chunkSize = 2097152,
    chunks = Math.ceil(file.size / chunkSize),
    currentChunk = 0,
    spark: any = new SparkMD5.ArrayBuffer(),
    fileReader = new FileReader();

  return new Promise((resolve: (params: any) => void, reject: (params: any) => void) => {
    fileReader.onload =  (e: any) => {
      spark.append((e.target as any).result);
      currentChunk++;
      if (currentChunk < chunks) {
        loadNext();
      } else {
        const md5Str: string =  spark.end();
        resolve(md5Str);
      }
    };

    fileReader.onerror =  () => {
      reject('加密失败');
    };

    function loadNext() {
      let start = currentChunk * chunkSize,
        end = ((start + chunkSize) >= file.size) ? file.size : start + chunkSize;
      fileReader.readAsArrayBuffer(blobSlice.call(file, start, end));
    }

    loadNext();
  })
}

```