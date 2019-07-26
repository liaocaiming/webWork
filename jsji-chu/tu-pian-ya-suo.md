```js
/*
 * path: 图片路径
 * qual: 压缩质量
 */
function canvasDataURL(path: string, qual?: number) {
  return new Promise((resolve: any, rejected: any) => {
    const img = new Image();
    const maxWidth = 750;
    img.onload = () => {
      // 默认按比例压缩
      const w = img.width;
      const h = img.height;
      let quality = 0.7; // 默认图片质量为0.7
      // 生成canvas
      const canvas: HTMLCanvasElement = document.createElement('canvas');
      const ctx: any = canvas.getContext('2d');
      // 最大尺寸限制
      // 创建属性节点
      if (w > maxWidth) {
        canvas.width = maxWidth;
        canvas.height = maxWidth * h / w;
      } else {
        canvas.width = w;
        canvas.height = h;
      }
      // canvas.width = w;
      // canvas.height = h;
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
      // ctx.drawImage(img, 0, 0, w, h);
      // 图像质量
      if (qual && qual <= 1 && qual > 0) {
        quality = qual;
      }
      // quality值越小，所绘制出的图像越模糊
      const newBase64Data = canvas.toDataURL('image/jpeg', quality);
      // 回调函数返回base64的值
      resolve(newBase64Data);
    };
    img.onerror = () => {
      rejected('图片加载失败');
    };
    img.src = path;
  });
}

// 图片压缩;
export default function imageZip(options: { file: any; quality?: number }) {
  const { file, quality } = options;
  return new Promise((resolve: any, rejected: any) => {
    const ready: any = new FileReader();
    ready.readAsDataURL(file);
    ready.onload = () => {
      const base64Url = ready.result;
      if (base64Url.indexOf('image/jpeg;') !== -1) {
        canvasDataURL(base64Url, quality).then(resolve);
      } else {
        resolve(base64Url);
      }
    };
    ready.onerror = () => {
      rejected('转换base64失败');
    };
  });
}
```