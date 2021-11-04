##### MVVM 

VUE 参考了mvvm架构模型，m（模型model）data中的数据，v（视图view）模版，vm（视图模型viewmodel）vue 实例对象

但是vue中添加了一个属性。ref
通过ref可以拿到dom对象，通过ref直接去操作视图。这一点上，违背了mvvm

##### object.difineProperty（数据代理）

Vue2.x 是使用 Object.defindProperty()，来进行对对象的监听的，但是改变数组里的某个值不会触发set，(如果要监听的到话，需要重新编写数组的方法)， 必须遍历每个对象的每个属性，如果对象嵌套很深的话，需要使用递归调用。



第一个参数对象 ：目标（给哪个对象添加属性）

第二个参数字符串 ：名字（属性名叫什么）

第三个参数类型是对象：配置项  {enumerable:true；控制属性是否可枚举，默认值false。writable:true;控制属性是否可修改，默认false。configurable:true;控制属性可删除，默认false。}

数据代理案例

```js
let a = {
  b:"1"
}
let c = "2"
Object.defineProprrty(a,"c",{
  get(){
    return c
  }
  set(value){
  return c = value
}
})
```



##### object.keys

遍历对象的属性名返回一个新数组