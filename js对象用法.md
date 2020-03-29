##对象基本操作方法

### 指定对象原型 然后继承

#### Object.setPrototypeOf(target，sources)

**target: 目标对象,sources: 源对象**

* 下列中obj1继承了obj2的属性，**解构可以解构到原型上的值**

```js
const obj1 = {}
const obj2 = {foo:"bar"}
Object.setPrototypeOf(obj1,obj2)

const {foo} = obj1
console.log(foo) //"bar"
```

#### Object.getPrototypeOf()

该方法与`Object.setPrototypeOf`方法配套，用于读取一个对象的原型对象

```js
Object.getPrototypeOf(obj); 
```

#### super指向当前对象的原型对象

```js
const proto = {   foo: 'hello' }; 
 
const obj = {   
    find() {     
        return super.foo;   
    } 
}; 

Object.setPrototypeOf(obj, proto);
obj.find() // "hello" 
// 上面代码中，对象obj的find方法之中，通过super.foo引用了原型对象proto的foo属性。
```

注意，super关键字表示原型对象时，**只能用在对象的方法之中，用在其他地方都会报错。**



###合并对象

语法: `Object.assign(target,sources)`   target: 目标对象,sources: 源对象

```js
const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };
const returnedTarget = Object.assign(target, source);

console.log(target); // Object { a: 1, b: 4, c: 5 }
console.log(returnedTarget); // Object { a: 1, b: 4, c: 5 }
```

### 对象的解构

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


### 创建对象的方式有哪些 

工厂模式、构造函数 直接扩号 或者new一个

## 对象属性描述介绍

###获取对对象属性的描述对象

#### Object.getOwnPropertyDescriptor()

```js
let obj = { foo: 123 };
console.log(Object.getOwnPropertyDescriptor(obj, 'foo'))
//打印结果
{
    configurable: true
    enumerable: true //为felsa时就是不可枚举属性
    value: 123  //键值对的值
    writable: true 
    __proto__: Object
}
```

**Object.assign()： 忽略enumerable为false的属性，只拷贝对象自身的可枚举的属性**



## 便利对象方法

###第一种：for...in  只遍历对象自身的和继承的可枚举的属性

```js
const obj = {
            id:1,
            name:'zhangsan',
            age:18
}

 for(let key  in obj){
        console.log(key + '---' + obj[key])
  }
```

###第二种：Object.keys（obj）和Object.values（obj）Object.entries()

Object.keys方法，返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键名

```js
var obj = { foo: 'bar', baz: 42 }; 
Object.keys(obj) // ["foo", "baz"] 
```

Object.values方法返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键

值。

```js
const obj = { 100: 'a', 2: 'b', 7: 'c' }; 
Object.values(obj) // ["b", "c", "a"] 
```

Object.entries方法返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键值对数组。

```js
const obj = { foo: 'bar', baz: 42 }; 
Object.entries(obj) // [ ["foo", "bar"], ["baz", 42] ]
```

### 第三种：使用Object.getOwnPropertyNames(obj)

返回一个数组，包含对象自身的所有属性（包含不可枚举属性）遍历可以获取key和value

```js
const obj = {
            id:1,
            name:'zhangsan',
            age:18
    }
    Object.getOwnPropertyNames(obj).forEach(function(key){
        console.log(key+ '---'+obj[key])
    })
```

###第四种：Reflect.ownKeys(obj).forEach()

```js
Reflect.ownKeys(obj).forEach(function(key){
    log(key,obj[key]);
});
// 结果如下
a 1
b 2
c 3
```

##Object.defineProperty()语法说明

数据描述符 --特有的两个属性`（value,writable）`

```csharp
let Person = {}
Object.defineProperty(Person, 'name', {
   value: 'jack',
   writable: true // 是否可以改变
})
```