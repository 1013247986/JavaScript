####对象原型上的继承Object.setPrototypeOf(target，sources)

**target: 目标对象,sources: 源对象**

* 下列中obj1继承了obj2的属性，**解构可以解构到原型上的值**

```js
const obj1 = {}
const obj2 = {foo:"bar"}
Object.setPrototypeOf(obj1,obj2)

const {foo} = obj1
console.log(foo) //"bar"
```



####Object.assign()用法讲解

**语法: Object.assign(target, …sources) target: 目标对象,sources: 源对象**

```js
const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };
const returnedTarget = Object.assign(target, source);

console.log(target); // Object { a: 1, b: 4, c: 5 }
console.log(returnedTarget); // Object { a: 1, b: 4, c: 5 }
```

#### 对象的解构

**声明的变量用于解构**

* 如果要将一个已经声明的变量用于解构，需要注意

```js
let x;
 {x} = {x:1};
// 会报错 正确写法如下
let x;
 ({x} = {x:1});
```

**对象解构设置默认值**

* 对象的解构可以指定默认值

  ```
  let {x = 3} = {};
  let {x,y=5} = {x = 1};
  ```

  