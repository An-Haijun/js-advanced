# 8.1 window 对象

BOM 的核心对象是 window 对象，它表示浏览器的一个实例。在浏览器中 window 对象有双重角色，它既是通过 JavaScript 的访问浏览器窗口的一个接口，又是 ECMAScript 规定的 Global 对象。这意味着在网页中定义的任何一个对象、变量和函数，都以 window 作为其 Global 对象，因此有权访问如 parseInt() 等方法。

## 8.1.1 全局作用域

由于 window 对象同时扮演着 ECMAScript 中 Global 对象的角色，因此所有在全局作用域中声明的变量、函数都会变成 window 对象的属性和方法。

```javascript
var age = 29;
function sayAge() {
    console.log(this.age);
}

console.log(window.age); // 29
sayAge();                // 29
window.sayAge();         // 29
```

以上代码，在全局作用域中定义了一个变量 age 和一个函数 sayAge()，他们被自动赋在 windoww 对象下。所以我们可以通过 window.age 和 window.sayAge() 访问它们。
由于 sayAge() 存在与全局作用域中，因此其函数内的 this.age 指向 window 对象，映射到 window.age。

定义全局变量与在window上直接定义属性有差别：全局变量不能通过 delete 操作符删除，而直接在 window 对象上定义的属性可以。

```javascript
var age = 29;
window.color = 'red';

// 在 IE < 9 时抛出错误，在其他所有浏览器中都返回 false
delete window.age;

// 在 IE < 9 时抛出错误，在其他所有浏览器中都返回 true
delete window.color; // return true

console.log(window.age); // 29
console.log(window.color); // undefined
```

另外尝试访问一个未定义的变量会抛出错误，但是通过查询 window 对象们可以知道某个可能为声明的变量是否存在。

```javascript
// 这里会抛出错误，变量 oldVal 未定义
var newVal = oldVal;

// 这里不会抛出错误，因为这是一次属性查询
var newVal = window.oldVal;
console.log(newVal); // undefined
```

**注：windows mobile 平台的 IE 浏览器不允许通过 window.prototype = value之类的形式，直接在 window 对象上创建新的属性和方法。可是，在全局作用域中声明的所有变量核函数，照样会变成 window 对象的成员。**

## 8.1.2 窗口关系及框架

框架标签：frameset、frame>

该小节暂不做总结，因为它在将来HTML5中会取消。替代方案推荐使用现代的单页框架（Vue、React和Angular）、ajax、node和php等。

## 8.1.3 窗口位置

用来确定和修改 window 对象位置的属性和方法有很多。IE、Safari、Opera 和 Chrome都提供了 ScreenLeft 和 ScreenTop。
