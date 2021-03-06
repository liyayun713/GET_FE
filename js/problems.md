# 一些有趣的js题目
### 1、知识点：
  * 词法分析期、执行期
  * 变量提升
```js
b();              // 'i am b'
console.log(a);   // undefined

var a = 'i am a';

function b () {
  console.log('i am b');
}
```
### 2、知识点：
  * 因为 b 是在全局环境中声明的，所以 value 的声明会在全局环境下寻找
```js
function b() {
  console.log(value);
}
function a() {
  var value = 2;
  b();
}

var value = 1;
a();    // 1
```
### 3、知识点：
  * 异步
  * 因为浏览器会有一个 Event Queue 存放异步通知，JS 在执行代码时会产生一个执行栈，同步的代码在执行栈中，异步的在 Event Queue 中。只有当执行栈为空时，JS 才会去 Event Queue 中查看是否有需要处理的通知，有的话拿到执行栈中去执行；
  * 并且 setTimeout 也有个小细节，第二个参数设置为 0 也许会有人认为这样就不是异步了，其实还是异步。这是因为 HTML5 标准规定这个函数第二个参数不得小于 4 毫秒，不足会自动增加
```js
function sleep() {
  var ms = 2000 + new Date().getTime();
  while(new Date() < ms) {};
  console.log('sleep finish');
}
document.addEventListener('click', function(){
  console.log('click');
});
sleep();
console.log('finish');
```
### 4、知识点：
  * JS 共有 6 个基本数据类型，分别为 Boolean, Null, Undefined, Number, String, Symbol，这些类型都是值不可变的；
  * 对于对象来说，直接将一个对象赋值给另外一个对象就是浅拷贝，两个对象指向同一个地址，其中任何一个对象改变，另一个对象也会被改变;
```js
var a = [1, 2]
var b = a
b.push(3)
console.log(a, b) // -> 都是 [1, 2, 3]
```
### 5、知识点：
  * this
  * 箭头函数中的this
```js
function foo() {
  console.log(this.a)
}
var a = 1;
foo();   // 1

var obj = {
  a: 2,
  foo: foo
}
obj.foo();    // 2

var c = new foo()
c.a = 3;
console.log(c.a);   // 3

// 箭头函数中的this指向
function a() {
    return () => {
        return () => {
            console.log(this)
        }
    }
}
console.log(a()()());   // window
```
### 6、知识点：
  * 我们只需要考虑函数是怎么调用的，这里符合上述第一个情况，所以应该是指向 window（调用setName()）
```js
var a = {
    name: 'js',
    log: function() {
        console.log(this)
        function setName() {
            this.name = 'javaScript'
            console.log(this)
        }
        setName()
    }
}
a.log();    // a、window
```
### 7、知识点：
  * 立即执行函数
  * 闭包
```js
function a() {
    var array = []
    for(var i = 0; i < 3; i++) {
        array.push(
            function() {
                console.log(i)
            }
        )
    }
    return array
}

var b = a()
b[0]()
b[1]()
b[2]()
// 3 3 3
```
### 8、知识点：
```js
function fun(n, o) {
  console.log(o);
  return {
    fun: function(m) {
      return fun(m, n);
    }
  }
}
fun(0).fun(1).fun(2);
let fn = fun(0).fun(1).fun;
console.log(fn);
fn(2);
fn(3);
fn(100).fun(2);
fn('a').fun(0);
```
