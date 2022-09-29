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



### 经典面试题

**let**

```javascript
console.log('=============经典面试题1=============');

var arr = [];

// 由于var是全局变量，所以i在最后会变成2，所以输出两个2
for(var i = 0; i < 2; i++){
    arr[i] = function(){
        console.log(i);
    }
}

arr[0]();
arr[1]();
```

