**数据转字符串**

JSON.stringify(deta,[]|function)：只串行化对象自身的可枚举的属性。

第二个参数可以传入数组或函数

传入数组如['key','name'] 就只会字符串化key和name
传入函数所有属性都会从函数里面过一遍



**字符串转对象**

JSON.parse()



**常用场景**

深拷贝的时候使用，如：JSON.parse(JSON.stringify(deta,[]|function))
注意不能拷贝函数，如果有函数，还是需要自己动手处理