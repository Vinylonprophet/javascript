## ES6-Notes

### 1. 类和方法

#### 面向对象编程介绍

**面向过程 (POP)：**分析出解决问题所需要的步骤，然后用函数把这些步骤一步步实现，使用的时候一次次调用

**面向对象(OPP)：** 把事务分解成一个个对象，然后由对象之间分工和合作

**特性:**

- ​	封装性
- ​	继承性
- ​	多态性

**面向过程**

​	优点：性能比面向对象高，适合和硬件紧密联系，比如单片机的面向过程

​	缺点：没有面向对象易维护，易复用，易扩展

**面向对象**

​	优点：易维护，易复用，易扩展，由于面向对象有封装，继承，多态的特性，可以设计出低耦合的系统，使系统更灵活，更易于维护

​	缺点：性能比面向过程低



#### ES6中的类和对象

世间的事务分具体的和抽象的事物

**思维特点**

1. 抽取(抽象)对象公用的属性和行为组织(封装)成一个类(模板)
2. 对类进行实例化，获取类的对象

**对象**

对象是一组无序的相关属性和方法的集合，所有事物都是对象

由`属性`和`方法`组成:

​	属性: 事物的特征，对象中用属性来表示

​	方法: 事物的行为，对象中用方法来表示

**类**

ES6中新增了类的概念，使用`class`声明一个类，之后用这个类进行实例化

`类`抽象了公共部分，泛指了一个大类

`对象`特指某一个，通过类实例化一个对象



#### 创建类和生成实例

**constructor**

constructor()方法是类的构造函数，`用于传递参数`，`返回实例对象`，通过new命令生成对象实例时，自动调用。如果没有显式定义，类内部会自动创建一个constructor

```javascript
// 1. 创建类
class Person{
	constructor(uname，age){
        this.uname = uname;
        this.age = age;
    }
}

// 2. 利用类new一个对象
new Person()
```

注意事项：

1. class关键字创建类，首字母大写
2. 类中由constructor，可以接受参数，同时返回实例对象
3. constructor只要有new生成实例，就会自动调用，如果不写constructor，类也会自动生成这个函数
4. 生成的new不能省略



**类添加方法**

```javascript
class Person{
	constructor(uname，age){
        this.uname = uname;
        this.age = age;
    }
    sing(song){
        console.log(this.uname + song)
    }
}
```

注意事项：

1. 类中的所有函数都不需要写function
2. 多个函数方法之间不需要添加逗号，分号



#### 类的继承

**基本语法**

```javascript
class Father{
    constructor(){

	}
    money(){
    	console.log(100)
	}
}

class Son extends Father{

}

var son = new Son();
son.money();
```

输出100



**super关键字**

super关键字用于访问和调用对象父类上的函数，`可以调用父类的构造函数`，也可以调用父类的普通函数

```javascript
class Father {
    constructor(x,y) {
        this.x = x;
        this.y = y;
    }
    sum(){
        console.log(this.x + this.y);
    }
}

class Son extends Father {
    constructor(x,y){
        this.x = x;
        this.y = y;
    }
}

var son = new Son(1,2);
son.sum();
```

上述由于Son写了一个constructor，其中的x和y，只传给自己，并没有传给父类，所以使用`this.x`和`this.y`的sum就无法被使用

代码更新：

```javascript
class Father {
    constructor(x,y) {
        this.x = x;
        this.y = y;
    }
    sum(){
        console.log(this.x + this.y);
    }
}

class Son extends Father {
    constructor(x,y){
        super(x,y);
    }
}

var son = new Son(1,2);
son.sum();
```



**super注意事项**

注意事项：

1. 继承中，如果实例化子类输出一个方法，先看子类有没有，有就先执行
2. 如果子类没有，就去父类查看有没有，有就执行（就近原则）
3. 也可以在子类中使用`super.function()`从而执行父类的方法
4. 如果想要扩展一个方法，那么一定要在构造函数中把super放在this之前，如下

```javascript
// 子类
class Son extends Father{
    constructor(x,y){
        super(x,y);
        this.x = x;
        this.y = y;
    }
    substract(){
        console.log(this.x - this.y);	//扩展
    }
}
```



#### 类的三个注意点

1. 在 ES6 中类没有变量提升，所以必须先定义类，才能通过类实例化对象
2. 类里面的共有属性和方法一定要加this使用
3. 类里的this指向问题
   1. constructor里面的this指向的是创建出来的实例对象
   2. 方法中this的指向是调用者（比如说某个按钮调用，那么this就是指向那个按钮）



### 2. 构造函数和原型

Class类是ES6之后新增的，ES6之前是通过构造函数和原型来模拟类的实现机制



#### 构造函数创建对象

**创建对象的三种方式**

1. 对象字面量
2. new Object()
3. 自定义构造函数

```javascript
// 1. 利用new Object()创建对象
var obj1 = new Object()

// 2. 利用对象字面量创建对象
var obj2 = {}

// 3. 利用构造函数创建对象
function Star(uname, age){
    this.uname = uname;
    this.age = age;
    this.sing = function(){
        console.log('唱歌');
    }
}

var ldh = new Star('刘德华', 18);
ldh.sing();
```



**构造函数**

new执行做的事情

1. 内存中创建一个新的空对象
2. 让 this 指向这个对象
3. 执行构造函数中的代码，给这个对象添加属性和方法
4. 返回这个对象`(所以构造函数不需要return)`



#### 静态成员和实例成员

构造函数的属性称之为成员，成员是可以添加的

- 实例成员

  1. 实例成员就是构造函数内部通过this添加的成员，name age sing 就是实例成员
  2. 实例成员只能通过实例化的对象来访问

- 静态成员

  1. 在构造函数上本身添加的函数
  2. 静态成员只能通过构造函数来访问

```javascript
// 添加
Star.sex = '男';
// 访问
console.log(Star.sex)
```



#### 原型对象prototype

**构造函数存在的问题**

构造函数创建对象时，如果内部有function的话，每个实例都会单独开辟一个function的内存空间，存在资源浪费



**构造函数原型prototype**

构造函数通过原型分配的函数是所有对象所共享的，每一个构造函数都有一个`prototype`属性，指向另一个对象

把不变的方法直接定义在prototype上，这样所有的对象实例就可以共享这些方法

```javascript
// 添加原型对象
Star.prototype.sing = function(){
    console.log("Sing a song");
}
```

注意事项：

1.  这个`prototype`就是一个对象，这个对象的所有属性和方法都会被构造函数所拥有
2.  一般情况下，我们的公共属性定义到构造函数里面，公共方法放在原型对象上



#### 对象原型 __ proto __

对象都会有一个属性 __ proto __ 指向构造函数的prototype原型对象，之所以可以用构造函数prototype原型对象的属性和方法，就是因为有 __ proto __ 的存在

```javascript
function Star(uname, age){
    this.uname = uname;
    this.age = age;
}
Star.prototype.sing = function(){
    console.log('Sing a song')
}
var ldh = new Star('刘德华', 18);
ldh.sing();
console.log(ldh);	// 对象（ldh）身上就有__proto__属性指向构造函数的原型对象
```



#### constructor 构造函数

prototype 和 __ proto __ 里都有constructor属性，我们称之为构造函数，因为他指回构造函数本身

```javascript
function Star(uname, age){
    this.uname = uname;
    this.age = age;
}
// 正确
Star.prototype.sing = function(){
    console.log('我会唱歌');
}
Star.prototype.movie = function(){
    console.log('我会演电影');
}

// 正确
Star.prototype = {
	constructor: Star,
    sing: function(){
    	console.log('我会唱歌');
    },
    movie: function(){
    	console.log('我会演电影');
    }
}

// 错误，因为Star的prototype被覆盖掉了，就没有constructor属性了
Star.prototype = {
    sing: function(){
    	console.log('我会唱歌');
    },
    movie: function(){
    	console.log('我会演电影');
    }
}
```



#### 原型链

**Picture**

![未命名绘图 (1)](C:\Users\尛丶\Downloads\未命名绘图 (1).png)



#### JavaScript的查找机制

1. 当访问一个对象属性（包括方法），先查找这个对象自身有没有
2. 如果没有就找他的原型（ __ proto __ 指向的构造函数的prototype 即：`原型对象`）
3. 如果还没有就找Object的原型对象，即：`Object.prototype`
4. 以此类推一直找到`null`为止
5.  __ proto __ 的意义就是为对象成员查找机制提供一个路线



#### 原型对象的this指向

1. 构造函数中，里面的this指向的是对象实例
2. 原型对象函数里面的this指向的也是对象实例

```javascript
function Star(uname, age){
	this.uname = uname;
	this.age = age;
}

var that;

Star.prototype.sing = function(){
	console.log('sing');
}

var ldh = new Star('刘德华', 18);

ldh.sing();

console.log(ldh === that);
```



#### 扩展内置对象

原型对象 扩展内置对象方法

```javascript
Array.prototype.sum = function(){
    var sum = 0;
    for(var i = 0; i < this.length; i++){
        sum += this[i];
    }
    return sum;
}
var arr = [1,2,3];
console.log(arr.sum());
```

注意事项：不能以对象的形式覆盖掉



### 3. 继承

ES6之前没有提供extends继承，通过`构造函数`和`原型对象`模拟继承，称之为`组合继承`



#### call()方法

call() 方法可以调用函数

```javascript
function fn(x, y){
    console.log('喝咖啡');
    console.log(this);
}

// 此处等价于fn()
// this指向window
fn.call();
```

call() 方法可以改变this的指向

```javascript
function fn(x, y){
    console.log('喝咖啡');
    console.log(this);
}

var o = {
	name: 'andy'
};

fn.call(o);
```



#### 父构造函数继承属性

```javascript
// 父构造函数
function father(uname, age){
    // this指向父的实例
    this.uname = uname;
    this.age = age;
}

// 子构造函数
function son(uname, age, score){
    // this指向子的实例
    father.call(this, uname, age);
    this.score = score;
}
```



#### 原型对象继承方法

```javascript
function Father(uname, age){
    // this指向父的实例
    this.uname = uname;
    this.age = age;
}

Father.prototype.money = function(){
	console.log('月薪',100000);
}

function Son(uname, age, score){
    // this指向子的实例
    father.call(this, uname, age);
    this.score = score;
}

// 这样修改会有问题，如果给子的prototype添加function，父也会有，这是因为父的原型对象的地址给了子，地址相同
// son.prototype = father.prototype;

// 创建了一个父的实例对象，把son的原型对象指向了父的实例对象，这时候父的实例对象的__proto__有个方法叫money
Son.prototype = new Father();

Son.prototype.exm = function(){
	console.log('考试');
}
// 指回原来的原型对象
Son.prototype.constructor = Son;

```



#### 类的本质

ES6之前通过构造函数实现继承，ES6之后通过类实现继承

1. 类的本质还是一个函数，类就是构造函数的另一种写法
2. 类的所有方法都定义在类的prototype上
3. 类创建的实例中也有 __ proto __ 指向类的prototype
4. ES6的类就是语法糖
5. 语法糖：便捷写法，简单理解



### 4. ES5 新增写法

#### forEach()方法

- value：每个元素
- index：索引
- array：数组本身

```javascript
var arr = [1,2,3];

arr.forEach(function(value, index, array){
    console.log(value);
    console.log(index);
    console.log(array);
})
```



#### filter()方法

filter()方法`创建一个新数组`，满足条件的元素返回，用于`筛选`

```javascript
 var arr = [12, 66, 33, 16];

 console.log(arr.filter(function(value, index, array){
     return value % 2 == 0
 }))
```



#### some()方法

some()方法用于检测数组中的元素是否满足指定条件，返回`布尔值`

注意事项：如果找到`第一个满足条件的元素`，就返回true，否则返回false

```javascript
var arr = ['red', 'pink', 'green', 'yellow'];

arr.some(function(value){
	return value === 'green';
})
```



#### some()和forEach()的区别

forEach()：不会终止迭代

```javascript
var arr = ['red', 'pink', 'green', 'yellow'];
arr.forEach(function(value){
    if(value === 'green'){
        console.log('找到了该元素');
        return true;
    }
    console.log(1)
})
```

some()：会终止迭代

```javascript
var arr = ['red', 'pink', 'green', 'yellow'];

arr.some(function(value){
    if(value === 'green'){
        console.log('找到了该元素');
        return true;
    }
    console.log(1)
})
```

filter()：不会终止迭代

```javascript
var arr = ['red', 'pink', 'green', 'yellow'];

arr.filter(function(value){
    if(value === 'green'){
        console.log('找到了该元素');
        return true;
    }
    console.log(1);
})
```



#### trim()方法

trim()方法会从一个字符串的两端删除空白字符串，他不影响字符串本身，返回的是一个新字符串

```javascript
var a = '   asd fg ';
console.log(a);

var b = a.trim();
console.log(b);

var input = document.querySelector('input');
var btn = document.querySelector('button');
var div = document.querySelector('div');
btn.onclick = function(){
    if(input.value.trim() === ''){
        div.innerHTML = '';
        alert('请输入内容');
    }else{
        console.log(input.value);
        div.innerHTML = input.value;
    }
}
```



#### Object.keys()方法

用于获取对象自身所有属性

```javascript
var obj = {
    id: 1,
    pname: '小米',
    price: 1999,
    num: 10
}

var arr = Object.keys(obj);

// 返回一个由属性名组成的数组
console.log(arr);

arr.forEach(function(value){
	console.log(value);
})
```



#### Object.defineProperty()方法

Object.defineProperty() 定义新属性或修改原有的属性



**使用方法**

```javascript
// obj: 目标对象
// prop: 必需, 需定义或修改的属性的名字
// descriptor: 目标所拥有的特性

// 第三个属性 descriptor 说明: 以{}的形式书写
// configuarble: 目标是否可以被删除或是否可以被再次修改特性, true | false, 默认false

Object.defineProperty(obj, prop, descriptor)
```



**obj**

```javascript
var obj = {
    id: 1,
    pname: '小米',
    price: 1999
}
```



**value**

```javascript
// value: 设置属性的值, 默认undefined
Object.defineProperty(obj, 'price', {
	value: 1000
})
```



**writable**

```javascript
// writable: 是否可以重写, true | false, 默认false
Object.defineProperty(obj, 'id', {
	writable: false
})
```



**enumerable**

```javascript
// enumerable: 目标是否可以被枚举, true | false, 默认false
Object.defineProperty(obj, 'price', {
    value: 1000,
    writable: false,
    enumerable: false
})

// 此处无法遍历，因为enumerable是false
console.log(Object.keys(obj));
```



**configuarble**

```javascript
// configuarble: 目标是否可以被删除或是否可以被再次修改特性, true | false, 默认false
Object.defineProperty(obj, 'address', {
    value: 'HangZhou',
    writable: true,
    enumerable: true,
    configurable: false
})

// 删除pname就不会报错
delete obj.pname;

// 删除address就会报错
delete obj.address;
```



### 5. 函数进阶

#### 函数的定义和调用

**定义方式**

1. 函数声明`function`关键字（命名函数）
2. 函数表达式（匿名函数）
3. new Function('参数1','参数2','函数体')

```javascript
// 1. 自定义函数（命名函数）
function fn(){

};

// 2. 函数表达式（匿名函数）
var fun = function(){

}

// 3. new Function('参数1','参数2','函数体')
var f = new Function('a','b','console.log(a+b)')
f(1,5);

// 4. 所有函数都属于Function的实例（对象）
console.log(fn instanceof Object);
console.log(fun instanceof Object);
console.log(f instanceof Object);
```

注意事项：

1. Function里面参数必须是字符串格式
2. 第三种方式执行效率低，也不方便书写，使用较少
3. 所有函数都是Function的实例（对象）
4. 函数也属于对象



**调用方式**

1. 普通函数
2. 对象的方法
3. 构造函数
4. 绑定事件函数
5. 定时器函数
6. 立即执行函数

```javascript
// 1.普通函数
function fn(){
	console.log('调用普通函数');
}
fn();
fn.call();

// 2.对象的方法
var o = {
    sayHi: function(){
        console.log('调用对象的方法');
    }
}
o.sayHi();

// 3.构造函数
function Star(){

}
// 通过new创建实例对象
new Star();

// 4.绑定事件函数
btn.onclick = function(){

}

// 5.定时器函数
setInterval(function(){

}, 1000);

// 6.立即执行函数，是自动调用的
(function(){
	console.log('立即执行函数');
})()
```



#### this指向

**普通函数**

```javascript
// 普通函数 指向window
function fn(){
	console.log('调用普通函数' + this);
}
window.fn();
```



**对象的方法**

```javascript
// 对象的方法
var o = {
    sayHi: function(){
    	console.log('调用对象的方法'+ this);
    }
}
o.sayHi();
```



**构造函数**

```javascript
// 构造函数 原型对象里的this也指向实例对象
function Star(uname){

}

Star.prototype.sing = function(){
	console.log('sing');
}

// 通过new创建实例对象
var ldh = new Star();
```



**绑定函数事件**

```javascript
// 绑定事件函数 指向的btn这个调用者
btn.onclick = function(){

}
```



**定时器函数**

```javascript
// 定时器函数 指向window
window.setInterval(function(){
	console.log('调用定时器函数' + this);
}, 1000);
```



**立即执行函数**

```javascript
// 立即执行函数 指向的是window
(function(){
	console.log('立即执行函数' + this);
})()
```



#### 改变this指向

**call()方法**

1. 第一个作用可以调用函数，改变函数内部的this指向
2. 第二个作用是通过改变this指向，实现`继承`

```javascript
// 第一个作用
var o = {
	name: 'Andy'
}

function fn(a,b){
    console.log(this);
    console.log(a + b);
}

fn.call();
fn.call(o, 1, 2);

// 第二个作用
function Father(uname, age, sex){
    this.uname = uname;
    this.age = age;
    this.sex = sex;
}

function Son(uname, age, sex){
	Father.call(this, uname, age, sex);
}

var son = new Son('Vinylon', 18, 'male');

console.log(son);
```



**apply()方法**

1. 调用函数，可以改变函数内部的this指向
2. 参数必须是`数组`（伪数组）

```javascript
// 使用方法
fun.apply(thisArg,[argsArray])

var o = {
    name: 'Andy'
}

function fn(arr){
    console.log(this);
    console.log(arr);
}

fn.apply(o, ['green']);
```

应用：借助数学内置对象求最大值

```javascript
Math.max.apply(Math, arr);
Math.min.apply(Math,arr);
```



**bind()方法**

1. `不会调用函数`，但是能改变this指向
2. 返回由指定的this值和初始化参数改造的原函数拷贝
3. 如果有的函数我们不需要立即调用，但是又想要改变函数内部指向

```javascript
// 使用方法
fun.bind(thisArg, arg1, arg2);

var o = {
    name: 'Andy'
}

function fn(){
    console.log(this);
}

// fn.bind()不会执行函数fn，相当于返回一个指向o的新函数
var f = fn.bind(o);

f();
```



**button设置定时器**

```javascript
var btn = document.querySelector('button');
btn.onclick = function(){
    this.disabled = true;

    // 此处的this就是指调用者
    console.log(this);

    setTimeout(function(){
        this.disabled = false;

        // 此处是定时器，指向window
        console.log(this);

    }, 300)
}
```

方法一：设置that

```javascript
var that;
var btn = document.querySelector('button');
btn.onclick = function(){

    this.disabled = true;

    that = this;
    console.log(that);

    setTimeout(function(){

        that.disabled = false;
        console.log(that);

    }, 300)
}
```

方法二：bind()绑定

```javascript
var btn = document.querySelector('button');
btn.onclick = function(){

this.disabled = true;

// 定时器的function没有立刻调用，所以可以bind一下btn
// 注意：bind(this)可以写成bind(btn)
setTimeout(function(){

    this.disabled = false;

    }.bind(this), 300)
}
```



**call apply bind 总结**

相同点

- ​	都可以改变this指向

不同点

1. call 和 apply 会调用函数，bind 不会调用函数
2. call 和 apply 传递的参数不一样，apply 传递数组

应用场景

1. call 用作继承
2. apply 和数组相关，如借助Math实现数组最大最小值
3. bind 不调用函数，但是还想改变this指向，如定时器



### 6. 严格模式

#### 基本概念

**概念**

JS 除了提供正常模式外，还提供了严格模式（strict mode）。ES5 的严格模式采用具有限制性 JavaScript 变体的一种方式，即在严格模式下运行 JS 代码

严格模式在 IE10 以上的浏览器才会支持，旧版本会忽略



**严格模式不同之处**

1. 消除了 JS 语法的一些不合理之处、不严谨之处，减少了怪异的行为
2. 消除代码运行的一些不安全之处，保证代码的安全
3. 提高编译器效率，增加运行速度
4. 禁用了 ECMAScript 的未来版本中可能会定义的一些语法，为未来新版本的 JavaScript 做好铺垫。比如一些保留字如：class，enum，export，extends，import，super 不能做变量名



#### 开启严格模式

严格模式可以应用到`整个脚本`和`个别函数`中，因此严格模式可以分为`为脚本开启严格模式`和`为函数开启严格模式`



**为脚本开启严格模式**

为整个脚本文件开启严格模式，需要在所有语句之前放一个特定语句 "use strict"  ;（或 'use strict' ;）

```javascript
// 开启严格模式
'use strict'

// 为了防止变量污染，通过立即执行函数开启独立作用域，也相当于给script开启严格模式
(function(){
	'use strict'
})()
```



**为函数开启严格模式**

```javascript
// 为某个函数开启严格模式
function(){
    'use strict';
    // 下面的代码按照严格模式执行
}
```



#### 严格模式中的变化

**变量规定**

- 正常模式
  - 一个变量没有赋值，默认全局变量
  - 声明变量
- 严格模式
  - 变量都必须先用var声明，然后使用
  - 声明变量，如：delete x; 是错误的

```javascript
'use strict'
var num = 10;
console.log(num);

delete num;
```



**this指向**

- 正常模式
  - 以前全局作用域函数的this指向window对象
  - 以前构造函数时不加new也可以调用，当普通函数，this指向全局
- 严格模式
  - 现在全局作用于函数的this指向`undefined`
  - 如果构造函数不加new调用，this会报错

```javascript
'use strict'

// 现在指向undefined
function fn(){
	console.log(this);
}
fn();

// 以前如果因为this指向window，所以就会不new就会存在window.sex
// 现在不加new会报错，因为是undefined
// 加了new就指向创建的实例
function Star(){
	this.sex = 'male';
}
Star();
```

注意事项：

1. 严格模式下的定时器中的this指向的还是window
2. 严格模式下的事件，对象还是指向调用者



**函数变化**

- 正常模式
  - 可以有重名的参数
  - 可以在非函数代码块中声明函数
- 严格模式
  - 不能有重名的参数
  - 函数必须声明在顶层，新版本的JavaScript会引入“块级作用域”（ES6已引入）。不允许在非函数的代码块内声明函数

```javascript
'use strict'

// 普通模式下两个参数可以同名
// 严格模式下两个参数不能同名
function fn(a, a){
    console.log(a + a);
}

fn(1,2);

// 函数必须声明在顶层，新版本的JavaScript会引入“块级作用域”（ES6已引入）不允许在非函数的代码块内声明函数
// 语法错误
if(true){
    function fn(){ };
    fn();
}

// 语法错误
for(var i = 0; i < 5; i++){
    function fn2(){ }
    fn2();
}

// 合法
function fn3(){
    function fn4(){ }
}
```



### 7. 高阶函数

#### 基本概念

`高阶函数`是对其他函数进行操作的函数，它`接收函数作为参数`或`将函数作为返回值输出`

```javascript
// 接受的一个函数作为参数
function fn(callback){
	callback&&callback();
}
fn(function(){alert('hi')})

// 将函数作为返回值输出
function fn(){
	return function(){}
}
fn();
```

注意事项：

1. 此时的fn就是一个高阶函数
2. 函数也是一种数据类型，同样可以作为参数，传递给另一个参数使用。比如回调函数

```javascript
// 回调函数 div移动变色
$('div').animate({left: 500}, function(){
	$('div').css("backgroundColor","purple");
})
```



#### 闭包

**变量作用域**

变量根据作用域的不同分为两种：`全局变量`和`局部变量`

1. 函数内部可以使用全局变量
2. 函数外部不可以使用局部变量
3. 当函数执行完毕，本作用域的局部变量会销毁



**基本概念**

`闭包（Closure）`指有权访问另一个函数作用域中`变量`的函数

也就是一个作用域可以访问另一个函数内部的局部变量

```javascript
// 闭包：fun这个函数作用域访问了fn的局部变量num
function fn(){
	var num = 10;
	
	function fun(){
    	console.log(num);
    }
    
    fun();
}

fn();
```



**闭包的主要作用**

作用：延升了变量的访问范围

```javascript
// fn外部的作用域访问fn内部的作用域
// 本来num会被销毁，但是return函数就延升了作用范围
function fn(){
	var num = 10;
	
    function fun(){
    	console.log(num);
	}
	
    // 返回fun这个函数
    // 也可以写成
    // return function(){
    //     console.log(num);
    // }
    return fun;
}

var f = fn();

f();
```



**案例1**

获得li的索引

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>案例</title>
</head>
<body>
    <ul class="nav">
        <li>榴莲</li>
        <li>臭豆腐</li>
        <li>炸汤圆</li>
        <li>蚂蚁上树</li>
    </ul>
    <script>
		// 存在问题，i=4时跳出循环，但是无论是点击还是定时器，i都是4，所以会存在问题        
        // 动态添加属性
        var lis = document.querySelector('.nav').querySelectorAll('li');
        for(var i = 0; i < lis.length; i++){
            lis[i].index = i;
            lis[i].onclick = function(){
				console.log(this);
                console.log(this.index);
            }
        }
        
        // 利用闭包的方式
        // 会存在内存泄漏问题
        for(var i = 0; i < lis.length; i++){
            // 利用for循环产生了四个立即执行函数
            // 立即执行函数成为小闭包，因为立即执行函数里任何一个函数都可以使用它的i变量
            (function(i){
                lis[i].onclick = function(){
                    console.log(i);
                }
            })(i)
        }
    </script>
</body>
</html>
```



**案例2**

循环中的setTimeout

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>案例</title>
</head>
<body>
    <ul class="nav">
        <li>榴莲</li>
        <li>臭豆腐</li>
        <li>炸汤圆</li>
        <li>蚂蚁上树</li>
    </ul>
    <script>
        // 存在问题，i=4时跳出循环，但是无论是点击还是定时器，i都是4，所以会存在问题
        var lis = document.querySelector('.nav').querySelectorAll('li');
        for(var i = 0; i < lis.length; i++){
            (function(i){
                setTimeout(function(){
                    console.log(lis[i].innerHTML);
                },300)
            })(i)
        }
    </script>
</body>
</html>
```



**案例3**

计算打车价格

题目：

- 打车起步价13元（3公里内），之后每多一公里增加5块钱，用户输入公里数就可以计算打车价格
- 如果有拥堵情况，总价格多收取10块钱拥堵费

```javascript
var car = (function(){
    var start = 13;
    var total = 0;
    return {
    	price: function(n){
        if(n <= 3){
            total = start;
        } else {
            total = start + (n - 3)*5
        }
    		return total;
		},
    	yd: function(flag){
    		return flag ? total + 10 : total;
    	}
    }
})()
console.log(car.price(5));
console.log(car.yd(true));
```

