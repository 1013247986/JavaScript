##**1、this指向问题**

this是函数中的一个指针，在普通函数里，谁调用指向谁！在ES6的箭头函数中，谁定义指向谁!

* call解决this指向问题    格式 function.call（point ，parameter，parameter）

* call第一个是新的this指向 ，后面是函数的参数，不需要可以不写！
* apply和call的区别在于apply参数以数组的方式传入：function.call（point ，[parameter,parameter]）
* bind 和上面两种方式的区别在于function.bind(point)(参数，参数);

**vue 公共函数调用 this指向问题！如封装ajax请求：**

公共样式util.js 

```js
export const GetAxios = function (url, data) {
    return new Promise((resolve, reject) => {
        this.$axios.get(api + url, {
            params: data
        }).then(res => {
            resolve(res)
        }).catch(err => {
            reject(err.message)
        });
    })
}
```

  直接调用会报错，因为this不会指向vue实列！this会是undefined

  **调用的vue组件里，我们用call解决了this的指向问题**

```js
import { GetAxios } from "../../util/util";

GetAxios.call(this, "divimage/obtain", {}).then(data => {
   this.obj = data.data;
});
```

**还可以用let _this = this 来用使用外面的this**

