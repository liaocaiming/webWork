```js
/**
 * 解决 IOS 使用  position: fixed 时的光标跑位
 * 解决思路：使用position: absolute + scrollTo 模拟 fixed
 *
 * @param {*} modalElem
 * @param {*} maskElem
 */

function focusBlur(modalElem: HTMLElement, maskElem: HTMLElement) {
  if (!modalElem) {
    return ;
  }

  let startTime: number, endTime: number;
  let inputDomArr = Array.from(modalElem.querySelectorAll('input'));
  let textareaDomArr = Array.from(modalElem.querySelectorAll('textarea'));
  let domArr = [...inputDomArr, ...textareaDomArr]

  window.scrollTo(0, 0);
  modalElem.style.position = 'absolute';
  maskElem.style.position = 'absolute';

  domArr.forEach((elem) => {
    elem.addEventListener('focus', () => {
      endTime = Date.now();
    });
    elem.addEventListener('blur', () => {
      startTime = Date.now();
      let timer = setTimeout(function() {
        if (Math.abs(endTime - startTime) > 30) {
          window.scrollTo(0, 0);
        }
        clearTimeout(timer);
      }, 40);
    });
  })
}
```