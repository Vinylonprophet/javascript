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