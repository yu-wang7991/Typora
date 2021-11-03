### TS类型

|  类型   |      例子       |               描述               |
| :-----: | :-------------: | :------------------------------: |
| number  |    1,-1,2.5     |             任意数字             |
| string  |  'hi',"hi",hi   |            任意字符串            |
| boolean |   True,false,   |              布尔值              |
| 字面量  |     其本身      |         变量的值就是本身         |
|   any   |        *        |             任意类型             |
| unknown |        *        |          类型安全的any           |
|  void   | 空值(undefined) |     没有值(或者画undefined)      |
| object  | {name:"孙悟空"} |            任意JS对象            |
|  array  |     [1,2,3]     |            任意JS数组            |
|  tuple  |      [4,5]      | 固定长度的数组，元素，TS新增类型 |
|  enum   |    enum{A,B}    |         枚举，TS新增类型         |

### 类型断言

第一种

```tsx
let somevalue : unknown = "this is a string";
let strLength : number = (<string>sonmevalue).length;
```

第二种

```tsx
let somevalue : unknown = "this is a string";
let strLength : number = (sonmevalue as string).length;
```

