### 1. 动画翻转:

```
<style>
  div {
    pespective: 400px;
    position: relative;
    width:100px;
    height:222px;
  }
  div span {
    position:absolute;
    transform-origin:50% 50%;
    transition: all 1s ease 0s;
  }
  div:hover span {
    transfom: rotateX(360)
  }
</style>
 <div>
    <span>wo</span>
 </div>
 ```