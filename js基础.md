JavaScript-判断数据类型

#### 1、typeof

```js
        console.log(typeof 1);//number
        console.log(typeof '1');//string
        console.log(typeof true);//boolean
        console.log(typeof undefined);//undefined
        console.log(typeof {});//object
        console.log(typeof null);//object
        console.log(typeof Symbol());//symbol
        console.log(typeof []);//object
        console.log(typeof new Date());//object
        console.log(typeof new RegExp());//object
        console.log(typeof new Error());//object
        console.log(typeof NaN);//number
```

【注意】
1、typeof一个数组的时候会返回object；
2、判断数据是否为数组可以使用Array原型上的.isArray()方法，返回一个布尔值，

```js
         var a = [1,2,3];
         console.log(typeof a);  //object
         console.log(Array.isArray(a));  //true
```

3、typeof null也会返回object；
4、typeof new Date()、typeof new RegExp()、typeof new Error()都会返回object;
5、typeof NaN会返回number，NaN也是Number的一种。

#### 2、instanceof

利用instanceof来判断A是否为B的实例，表达为A instanceof B，返回一个布尔值。instanceof的原理是通过检测对象的原型链上是否含有类型的原型。

```js
        console.log({} instanceof Object);//true
        console.log([] instanceof Array);//true
        console.log([] instanceof Object);//true
        let fn = function (a, b) {
            return a + b
        }
        console.log(fn instanceof Function);//true

```

【注意】
面试中一个非常常见的手写代码题目就是手写instanceof，判读的思路就是看右边变量的原型是否存在于左边变量的原型链上。代码如下：

```js
        // 手写实现instanceof
        function myinstanceof(left, right) {
            let leftValue = left.__proto__;
            let rightValue = right.prototype;
            while (true) {
                if (leftValue === null) {
                    return false
                } else if (leftValue === rightValue) {
                    return true
                } else {
                    leftValue = leftValue.__proto__
                }
            }
        }
```

#### 3、constructor

JS规定，每个构造函数都会有一个prototype属性，即为构造函数的原型对象，而原型对象中会有一个constructor属性指回到构造函数。当利用构造函数创建新对象时，原型上的constructor属性也会被遗传到新创建的对象上，从原型链的角度讲，构造函数也代表了对象的类型。

```js
        console.log(new Number(1).constructor == Number);//true
        console.log(new String(1).constructor == String);//true
        console.log(true.constructor == Boolean);//true
        console.log(new Object().constructor == Object);//true
        console.log(new Error().constructor == Error);//true
```

#### 4、Object.prototype.toString.call()

toString()方法是Object原型上的方法，调用此方法，返回格式为[object,xxx]，xxx即为判断的结果。对于Object对象可以直接调用Object.prototype.toString()，对于其他数据类型，需要通过.call()来调用。

```js
        console.log(Object.prototype.toString({}));//[object Object]
        console.log(Object.prototype.toString.call(''));//[object String]
        console.log(Object.prototype.toString.call(1));//[object Number]
        console.log(Object.prototype.toString.call(true));//[object Boolean]
        console.log(Object.prototype.toString.call(undefined));//[object Undefined]
        console.log(Object.prototype.toString.call(null));//[object Null]
        console.log(Object.prototype.toString.call(Symbol()));//[object Symbol]
        console.log(Object.prototype.toString.call(new Error()));//[object Error]
```

