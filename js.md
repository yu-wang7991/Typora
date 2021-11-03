### JavaScript-判断数据类型

1、typeof

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

2、instanceof

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

3、constructor

JS规定，每个构造函数都会有一个prototype属性，即为构造函数的原型对象，而原型对象中会有一个constructor属性指回到构造函数。当利用构造函数创建新对象时，原型上的constructor属性也会被遗传到新创建的对象上，从原型链的角度讲，构造函数也代表了对象的类型。

```js
        console.log(new Number(1).constructor == Number);//true
        console.log(new String(1).constructor == String);//true
        console.log(true.constructor == Boolean);//true
        console.log(new Object().constructor == Object);//true
        console.log(new Error().constructor == Error);//true
```

4、Object.prototype.toString.call()

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

### 箭头函数

> 箭头函数体内的`this`对象，就是定义**该函数时所在的作用域指向的对象**，而不是使用时所在的作用域指向的对象。

下面是普通函数的列子：

```js
var name = 'window'; // 其实是window.name = 'window'

var A = {
   name: 'A',
   sayHello: function(){
      console.log(this.name)
   }
}

A.sayHello();// 输出A

var B = {
  name: 'B'
}

A.sayHello.call(B);//输出B

A.sayHello.call();//不传参数指向全局window对象，输出window.name也就是window
```

从上面可以看到，sayHello这个方法是定义在A对象中的，当当我们使用call方法，把其指向B对象，最后输出了B；可以得出，sayHello的this只跟使用时的对象有关。

改造一下：

```js
var name = 'window'; 

var A = {
   name: 'A',
   sayHello: () => {
      console.log(this.name)
   }
}

A.sayHello();// 还是以为输出A ? 错啦，其实输出的是window
```

我相信在这里，大部分同学都会出错，以为sayHello是绑定在A上的，但其实它绑定在window上的，那到底是为什么呢？

一开始，我重点标注了“**该函数所在的作用域指向的对象**”，作用域是指函数内部，这里的箭头函数，也就是sayHello，所在的作用域其实是最外层的js环境，因为没有其他函数包裹；然后最外层的js环境指向的对象是winodw对象，所以这里的this指向的是window对象。

那如何改造成永远绑定A呢：

```js
var name = 'window'; 

var A = {
   name: 'A',
   sayHello: function(){
      var s = () => console.log(this.name)
      return s//返回箭头函数s
   }
}

var sayHello = A.sayHello();
sayHello();// 输出A 

var B = {
   name: 'B';
}

sayHello.call(B); //还是A
sayHello.call(); //还是A
```

OK，这样就做到了永远指向A对象了，我们再根据“**该函数所在的作用域指向的对象**”来分析一下：

1. **该函数所在的作用域：**箭头函数s 所在的作用域是sayHello,因为sayHello是一个函数。
2. **作用域指向的对象：A.**sayHello指向的对象是A。

所以箭头函数s 中this就是指向A啦 ～～

----------------finish-------------

最后是使用箭头函数其他几点需要注意的地方

1. 不可以当作构造函数，也就是说，不可以使用`new`命令，否则会抛出一个错误。
2. 不可以使用`arguments`对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。
3. 不可以使用`yield`命令，因此箭头函数不能用作 Generator 函数。

### class类

class中的constructor（构造函数）this表示当前的实例可以通过this向新建的对象中添加属性

```tsx
class dog {
  name:string;
  age:number;
  constructor(name:string,age:number){
    this.name = name;
    this.age = age;
  }
}
const dog  = new dog(name:'旺财',age:3)
const dog1 = new dog(name:'小黑',age:4)
```

这样可以用一个类创建不同的对象

#### 类的继承

extends

```tsx
class Animal {
  name:string;
  age:number;
  constructor(name:string,age:number){
    this.name = name;
    this.age = age;
  }
  sayHello(){
    console.log('你好')
  }
}
class dog extends Animal{ //拿到Animal（父类）所有的属性和方法
  saybye(){  //ocp 开闭原则
    
  },
  sayHello(){//覆盖父类（方法重写）  ocp 开闭原则
    console.log('你好呀')
  }
}
```

#### super()

如果在子类中写了construtor（相当于重写），必须对父类的construtor进行调用

```tsx
class Animal {
  name:string;
  constructor(name:string){
    this.name = name;
  }
  sayHello(){
    console.log('你好')
  }
}
class dog extends Animal{
  age:number;
  constructor(name:string,age:number){
    super(name)// 调用父类的构造函数
    this.age = age;
  },
  seyHellow(){
    super.sayHello()// 在类的方法中super就代表当前类的父类
    
  }
}
```

