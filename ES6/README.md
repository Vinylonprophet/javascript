## ES6-Notes

### let关键字

ES6新增的用于声明变量的关键字

- let声明的变量只在所处于的块级有效，var不具备这个特点
- 防止循环变量变成全局变量
- 不存在变量提升
- 暂时性死区



**基本使用**


```javascript
// let关键字是可以声明变量的
if(true){
    let a = 10;
}
console.log(a);     // a is not defined

if(true){
    let b = 20;
    console.log(b);
    if(true){
        let c = 30;
    }
    console.log(c); // 访问不到
}
```



**特点一**

防止循环变量变成全局变量

```javascript
// 如果是 var i = 0 ，for循环外可以访问得到
// let就是和for循环进行了绑定
for(let i = 0; i < 2; i++){

}
console.log(i);
```



**特点二**

不存在变量提升

```javascript
// 大概就是先使用，再声明，属于变量提升
console.log(a);

let a = 20;         // a is not defined
// var a = 20;      // undefined 但是变量提升了
```



**特点三**

暂时性死区

```javascript
var tmp = 123;

// 由于let是块级有效，所以let的tmp和外部的tmp没有关系，但是块级的let之前使用了tmp，所以会报错，报没有声明的错误
if(true) {
    tmp = 'abc';
    let tmp;
}
```



**经典面试题**

**let**

```javascript
console.log('=============var=============');

var arr = [];

// 由于var是全局变量，所以i在最后会变成2，所以输出两个2
for(var i = 0; i < 2; i++){
    arr[i] = function(){
        console.log(i);
    }
}

arr[0]();
arr[1]();

console.log('=============let=============');

var arr = [];

// let在循环的时候产生了两个块级作用域，输出时去对应的块级作用域中查找对应的i
for(let i = 0; i < 2; i++){
    arr[i] = function(){
        console.log(i);
    }
}

arr[0]();
arr[1]();
```



### const关键字

作用：声明常量，常量就是值（内存地址）不能变化的量

- 具有块级作用域
- 声明常量时必须赋值
- 常量赋值后，值不能修改



**特点一**

声明常量时必须赋值

```javascript
// 声明常量时必须赋值
var a;
let b;
const c;	// Missing initializer in const declaration
```



**特点二**

常量赋值后，值不能修改

```javascript
const PI = 3.14;
PI = 100;       // Assignment to constant variable

const arr = [100, 200];
arr[0] = 'a';
arr[1] = 'b';

console.log(arr);   // 能输出，因为内存地址没有改变
arr = ['a', 'b'];   // 重新赋值就是新数组，会改变内存地址，Assignment to constant variable
```

注意事项：如果是数组，会发生改变，因为`内存地址`没有发生改变



### let、const、var 的区别

1. 使用var声明的变量，作用域为该语句所处的函数内，存在变量提升现象
2. 使用let声明的变量，作用域为该语句所在的代码块内，不存在变量提升
3. 使用const声明的常量，后面代码中不能再修改该常量的值

| var          | let            | const          |
| ------------ | -------------- | -------------- |
| 函数级作用域 | 块级作用域     | 块级作用域     |
| 变量提升     | 不存在变量提升 | 不存在变量提升 |
| 值可更改     | 值可更改       | 值不可更改     |



### 数组解构赋值

ES6允许从数组中提取值，按照对应位置，对变量赋值。对象也可以实现解构



**数组解构**

数组解构允许我们按照一一对应的关系从数组中提取值然后将值赋值给变量

```javascript
// a, b, c 要一一对应
let [a, b, c] = [1, 2, 3];

console.log(a);
console.log(b);
console.log(c);

// 数量没对应上，foo就会是undefined
let [bar, foo] = [1];

console.log(bar);
console.log(foo);
```



**对象解构**

对象结构允许使用变量的名字匹配对象的属性，匹配成功将对象的属性的值赋值给变量

```javascript
// 方法一
let person = {
    name: '张三',
    age: '20'
}
let {name, age} = person;
console.log(name);

// 方法二
let person = {
    name: '张三',
    age: '20'
}
let {name: Myname, age: Age} = person;
console.log(Myname);
```



### 箭头函数

ES6新增的定义函数的方式，用来简化函数定义语法

- 函数体中`只有一句代码`，代码的执行结果就是返回值，`可以省略大括号`
- 如果形参`只有一个`，可以省略小括号
- 箭头函数不绑定this关键字，箭头函数中的this，指向的是`函数定义位置的上下文this`

```javascript
() => {}
const fn = () => {}
```



**特点一**

函数体中`只有一句代码`，代码的执行结果就是返回值，`可以省略大括号`

```javascript
const sum = (num1, num2) => num1 + num2;

console.log(sum(2, 4));
```



**特点二**

如果形参`只有一个`，可以省略小括号

```javascript
const fn = v => v;
```



**特点三**

箭头函数不绑定this关键字，箭头函数中的this，指向的是`函数定义位置的上下文this`

```javascript
// 箭头函数不绑定this关键字
const obj = {
    name: '张三'
}
function fn(){
    console.log(this);

    // 这个指向的是上文的this
    return () => {
        console.log(this);
    }

    // 这个指向的是window
    return function(){
        console.log(this);
    }
} 
const resFn = fn.call(obj)
resFn();
```



**经典面试题**

对象中方法的this指向

```javascript
var obj = {
    age: 20,
    say: () => {
        console.log(this);
        console.log(this.age);
    },
    run: function(){
        console.log(this);
        console.log(this.age);
    }
}

// 由于箭头函数指向箭头函数定义区域的this，obj身为一个对象无法产生作用域，所以实际上被定义在全局作用域下
obj.say();
```



### 剩余参数

剩余参数语法允许我们将一个不定数量的参数表示为一个数组

```javascript
function sum(first, ...args) {
    console.log(first);         // 10
    console.log(args);          // [20, 30]
}
sum(10, 20, 30);

const sum = (...args) => {
    let total = 0;
    args.forEach(item => {
        total += item;
    })
    return total;
}

console.log(sum(10, 20));
console.log(sum(10, 20, 30));
```



### 扩展运算符

**Array的扩展方法—扩展运算符（展开语法）**

- 扩展运算符可以将数组或者对象转为用`逗号分隔`的参数序列
- 扩展运算符可以应用于`合并数组`

```javascript
console.log('=====分割=====');
let ary = [1, 2, 3];
// 是有逗号的
...ary                      // 1, 2, 3,

// log出来本身就没有逗号
console.log(ary);
console.log(...ary);        // 1 2 3
console.log(1,2,3);         // 1 2 3

console.log('=====合并=====');

// 方法一
let ary = [1, 2, 3];
let arr = [4, 5, 6];
let ay = [...ary, ...arr];

console.log(ay);

// 方法二
let ary = [1, 2, 3];
let arr = [4, 5, 6];

ary.push(...arr);
console.log(ary);
```

