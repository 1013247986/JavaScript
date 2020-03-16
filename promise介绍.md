####promise介绍

* 1.promise本身是同步的

```js
let oP = new Promise( (res, rej) => {
     console.log(1);
});
console.log(2);

//1
//2
```

* promise的回调then是异步的

```js
let oP = new Promise( (res, rej) => {
     console.log(1);
});
oP.then(res => {
     console.log(res);
});
console.log(2);

//1
//2
//3
```



**promise执行介绍**

new 的promise会马上执行 

