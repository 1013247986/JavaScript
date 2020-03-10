## JS设计模式

[原文链接](https://blog.csdn.net/njj5210/article/details/80270147)

**单例模式，单体模式，工厂模式，策略模式，模版模式，观察-订阅者模式，外观模式**

#### 1、单体模式（常用的防止变量污染的设计模式）

单体模式比较好理解，它是一个对象，我们创建这个对象的目地是为了系统管理代码中的一些公用的变量，对象，函数，避免出现变量污染，一般我们声明一些变量和函数的时候，我们是在封闭的函数中，一般不会造成变量污染情况，但是以防万一，原因是在js中，全局变量和局部变量的关系比较复杂（有时候还夹杂着一部分变量提升问题），不少coder经常会因为此问题出现bug老半天找不到原因，创建好这个对象以后我们只是对外暴露一个对象入口，使用里面的变量时我们可以以object.变量名的形式来调用变量，其实也是为了实现js代码块的划分命名空间来设计的


```js
var Singleton = {
    attribute:true,
    method1:function(){},
　　 method2:function(){}
   }; 
```

####2、单例模式（常用的单例模式实例是弹出框）

单例模式的概念，是指单个对象，并且此对象有且只有一个实例化对象，保证一个类只有一个对应的实例化对象，在实例化前会进行判断，如果已经有了实例化对象直接return，否则才会去实例化对象出来，这样做是为了保证一些逻辑性的交互的存在，举个简单的栗子，登陆弹出框，假如我们现在要做一个登陆弹出框，我们在点击登陆按钮的时候，弹出框要弹出来，我们再次点击按钮，并不会再次弹出一个登录框（不排出有些奇葩会去做弹出多个登录框，让用户一个一个去关闭的反人类交互），要想实现单例模式，并不轻松，我们可以使用闭包来实现。常用的单例模式是全局本地缓存

```js
  var single = (function(){
    var unique;
    function getInstance(){
　　　　// 如果该实例存在，则直接返回，否则就对其实例化
        if( unique === undefined ){
            unique = new Construct();
        }
        return unique;
    }
    function Construct(){
        // ... 生成单例的构造函数的代码
    }
    return {
        getInstance : getInstance
    }
    })();
```

#### 3、策略模式（遵循对修改关闭，对扩展开放的原则）

策略模式主要是将算法的使用和算法的实现分离开，便于代码的维护，避免经常性的修改源代码，只需要添加新代码即可，举个栗子，超市卖东西，vip0.5折，普通用户没有折扣， 不使用策略模式，我们只能写一个条件判断语句来区分用户身份

```js
function Price(personType, price) {
    //vip 5 折
    if (personType == 'vip') {
        return price * 0.5;
    } else {
        return price; //其他都全价
    }
    } 

这么写看似没有什么问题，但是如果我们要添加用户身份呢，或者我们要频繁去修改折扣呢，我们需要不断的修改if else中的判断条件了，使用了策略模式，看似代码量多了

    // 对于vip客户
   function vipPrice() {
    this.discount = 0.5;
   }
 
    vipPrice.prototype.getPrice = function(price) {
　　return price * this.discount;
    }
// 对于普通客户
function Price() {
    this.discount = 1;
}
 
Price.prototype.getPrice = function(price) {
    return price ;
}

// 上下文，对于客户端的使用
function Context() {
    this.name = '';
    this.strategy = null;
    this.price = 0;
}
Context.prototype.set = function(name, strategy, price) {
    this.name = name;
    this.strategy = strategy;
    this.price = price;
}
Context.prototype.getResult = function() {
    console.log(this.name + ' 的结账价为: ' + this.strategy.getPrice(this.price));
}

var context = new Context();
var vip = new vipPrice();
context.set ('vip客户', vip, 200);
context.getResult();   // vip客户 的结账价为: 100

var Price = new Price();
context.set ('普通客户', Price, 200);
context.getResult();  // 普通客户 的结账价为: 200
```



#### 4、发布-订阅模式（Vue的双向绑定原理就是基于此模式）
