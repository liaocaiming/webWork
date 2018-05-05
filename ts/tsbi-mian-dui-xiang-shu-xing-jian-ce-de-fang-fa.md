## 方法一:
```
var obj = {a: 1}
Object['values'](obj)
```

## 方法二:

```
var obj = {b : 1}
(Object as any).values(obj)
```
## 方法三:
```
var obj = {c: 3}
(<any>Object).values(obj)
```