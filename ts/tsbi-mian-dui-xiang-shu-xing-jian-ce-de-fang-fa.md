## 方法一:
```js
var obj = {a: 1}
Object['values'](obj)
```

## 方法二:

```js
var obj = {b : 1}
(Object as any).values(obj)
```
## 方法三:
```js
var obj = {c: 3}
(<any>Object).values(obj)
```