## 数组方法

####1、数组转字符，字符转数组

```js
let arr = [1,2,3]
arr.toString();
'1,2,3'
arr.valueOf();
[1,2,3]
```

####2、伪数组转真数组

```js
//第一种
Array.prototype.slice.call(li)
//第二种
[...li]
```

####3、获取最大值

```js
//第一种方法
let arr=[45,552,3841]
Math.max(...arr)

//第二种
//二分法

//es5
//第三种
Math.max.apply(arr)
```

####4、合并数组

```js
const arr1 = ["a","b"]
const arr2 = ["c"]
const arr3 = ["d","e"]

//第一种办法
arr1.concat(arr2,arr3)
//["a","b","c","d","e"]

//第二种
[...arr1,...arr2,...arr3]
//["a","b","c","d","e"]
```

####5、将[[1,2],[3,4]]降维一维数组

```js
 Array.prototype.concat.apply([], arr);
```

####6、转换为真正数组 注：slice为返回选定的值，不包含第二个数！

```js
Array.prototype.slice.call(obj);
```

注：splice 写法  创建一个新数组，并向其添加一个元素

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];fruits.splice(2,0,"Lemon","Kiwi"); // 其中0代表添加

输出结果：
Banana,Orange,Lemon,Kiwi,Apple,Mango
```
