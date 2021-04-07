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

### Object.is(value1,value2) 判断两个值是否相等

列：解决了NaN不等于自身，完善-0和+0相等的问题   相等为true 不相等为false



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
    writable: true // 能否修改值
    __proto__: Object
}
```

**Object.assign()： 忽略enumerable为false的属性，只拷贝对象自身的可枚举的属性**



## 遍历对象方法

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

## Object.defineProperty(obj ，prop ，desc)语法说明


1. obj 需要定义属性的当前对象
2. prop 当前需要定义的属性名
3. desc 属性描述符

**数据描述符 --特有的两个属性`（value,writable）`**

* `writable: false` // 是否可以改变 数据描述符
* `configrable :true` // 描述属性是否配置，以及可否删除
* `value: ? `//属性值
* `delete Person.name`  //删除属性
* `enumerable ：true `// 是否出现在for in或Object.keys()遍历中，自身是否可枚举

```js
let Person = {}
Object.defineProperty(Person, 'name', {
   value: 'jack',
   writable: true, // 是否可以改变 数据描述符
   configrable :false // 描述属性是否配置，以及可否删除 可否属性定义修改
})
 delete Person.name   //Cannot delete property 'name' of #<Object>

Object.defineProperty(Person, 'name',{ //也不能重新定义属性
    value:'rose'    //Cannot delete property：name
})
```

当writable为false时可以通过属性定义的方式修改name属性值

```js
Object.defineProperty(Person, 'name', {
   value: 'jack',
   writable: false, // 是否可以改变 数据描述符
   configrable :true, // 描述属性是否配置，以及可否删除 可否属性定义修改
   enumerable :true // 自身是否能被枚举
})

Object.defineProperty(Person, 'name',{
    value:'rose' 
})
console.log(Person.name) // rose
```



#### configurable属性总结：

**configurable: false 时，不能删除当前属性，且不能重新配置当前属性的描述符(有一个小小的意外：可以把writable的状态由true改为false,但是无法由false改为true),但是在writable: true的情况下，可以改变value的值**

**configurable: true时，可以删除当前属性，可以配置当前属性所有描述符。**

###注意事项

```js
let Person = {}

person.gender = "male"
//等价于
Object.defineProperty(Person,"gender",{
    value:"male", //  属性值
    configurable:true, // 能否被删除 可否属性定义修改
    writable:true, //   是否可以改变
    enumerable:true //  自身可否被枚举
})
```



```js
Object.defineProperty(Person,"age",{
    value:"26", //  属性值
})
//等价于
Object.defineProperty(Person,"gender",{
    value:"26", //  属性值
    configurable:false, // 能否被删除 可否属性定义修改
    writable:false, //   是否可以改变
    enumerable:false //  自身可否被枚举
})
```

### 不变性

####对象常量
结合writable: false 和 configurable: false 就可以创建一个真正的常量属性（不可修改，不可重新定义或者删除）

####禁止扩展
如果你想禁止一个对象添加新属性并且保留已有属性，就可以使用`Object.preventExtensions(...)`

```js
let Person={
  name:'Jack'
}
Object.preventExtensions(Person)
Object.gender = 'male' // Can`t add Property gender,Object is not extensible
console.log(Person.gender)
```

   **但是可以配置**

```js
let Person={
  name:'Jack'
}
Object.preventExtensions(Person) // 禁止扩展
// 但是仍然可以进行配置
Object.defineProperty(Person,"name",{
    value:"rose", //  属性值
    configurable:false, // 能否被删除 可否属性定义修改
    writable:false, //   是否可以改变
})
// 输出配置的结果
console.log(Person.name)   //rose
// 不能进行扩展
Person.gender = 'male'
console.log(Person.gender) //undefined
```

**在非严格模式下，创建属性gender会静默失败，在严格模式下，将会抛出异常**

####密封
`Object.seal()`会创建一个密封的对象，这个方法实际上会在一个现有对象上调用`object.preventExtensions(...)`并把所有现有属性标记为`configurable:false`。

```js
let Person = {
  name:'Jack'
}
Object.seal(Person)
Person.gender = 'male'
// 不能扩展属性
console.log(Person.gender) // undefined
// 再次验证
console.log(Object.keys(Person)) ['name']
// 不能再次配置属性
Object.defineProperty(Person,'name',{ //Can`t add Property:name
    name:'rose',
    configurable:true
})
```

**所以， 密封之后不仅不能添加新属性，也不能重新配置或者删除任何现有属性（虽然可以改属性的值）**

####冻结

```js
let Person = {
  name:'Jack'
}
Object.freeze(Person)
Person.gender = 'male'
 // 不能扩展属性
console.log(Person.gender) // undefined
 // 再次验证
console.log(Object.keys(Person)) ['name']
Person.name='Tom'
 // 不可修改已有属性的值
console.log(Person.name) // Jack
 // 不能再次配置属性
Object.defineProperty(Person,'name',{ //Can`t add Property:name
    name:'rose',
    configurable:true
})
```

* 这个方法是你可以应用在对象上级别最高的不可变性，它会禁止对于对象本身及其任意直接属性的修改（但是这个对象引用的其他对象是不受影响的）

####重新定义属性名

```js
let proto = Object.defineProperties({},{
  foo:{
     value:'a',
     writable:false,
     // 只读
     configurable:true
     // 可配置
  }
})
let obj = Object.create(proto)
console.log(obj) // {}
Object.defineProperty(obj.'foo',{
    value:'b' // 通过自定义的形式创建了自身属性               
})
console.log(obj.foo) // b
console.log(obj.foo) // a
obj.foo = 'hello' // Cannot assign to read only property 'foo' fo object
```

赋值运算符不会改变原型链上的属性
不能通过为`obj.foo`赋值来改变`proto.foo`的值。这种操作只会在`obj`上新建一个自身属性

```js
let obj = {
  name:'Jack'
}
// 等同于
let obj = new Object()
Object.defineProperties(obj,{
    name:{
        value:'Jack',
        enumerable:true,
        configurable:true,
        writable:true
    }
})
```

记住以下两种形式的区别：

```js
let obj = {}
obj.name='Jack'

// 上面的代码等同于

let obj = {}
Object.defineProperty(obj,'name',{
        value:'Jack',
        enumerable:true,
        configurable:true,
        writable:true
})

// 另一方面
Object.defineProperty(obj,'name',{
        value:'Jack'
})

// 上面的代码等同于

Object.defineProperty(obj,'name',{
        value:'Jack',
        enumerable:false,
        configurable:false,
        writable:false
})
```

