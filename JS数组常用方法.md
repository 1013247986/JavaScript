## 数组方法

**1、数组转字符，字符转数组**

```js
let arr = [1,2,3]
arr.toString();
'1,2,3'
arr.valueOf();
[1,2,3]
```

**2、伪数组转真数组**

```js
//第一种
Array.prototype.slice.call(li)
//第二种
[...li]
```

**3、获取最大值**

```js
//第一种方法
let arr=[45,552,3841]
Math.max(...arr)
//第二种
//二分法
```

