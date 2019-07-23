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

用来确定和修改 window 对象位置的属性和方法有很多。IE、Safari、Opera 和 Chrome都提供了 ScreenLeft 和 ScreenTop，分别用于表示窗口相对于屏幕左边和上边的位置。

| -       | screenLeft | screenTop | screenX | screenY |
|---------|------------|-----------|---------|---------|
| IE      | y          | y         | n       | n       |
| Safari  | y          | y         | y       | y       |
| Opera   | y          | y         | y-      | y-      |
| Chrome  | y          | y         | y       | y       |
| Firefox | n          | n         | y       | y       |

注：'y-' 表示属性支持但并不对应。

**下面代码可以跨浏览器去的窗口左边和上边对应的位置：**

```javascript
var leftPos = (typeof window.screenLeft === 'number') ?
                window.screenLeft : window.screenX;

var topPos = (typeof window.screenTop === 'number') ?
                window.screenTop : window.screenY;
```

**将窗口精确移动到某一位置，moveTo() 和 moveBy()：**

- 都接收两个参数；
- moveTo()：新位置的 x 和 y 坐标值；
- moveBy()：水平和垂直方向上移动的像素值。

## 8.1.4 窗口大小

跨浏览器确定一个窗口的大小不是一件简单的事。IE9+、Firefox、Safari、Opera 和 Chrome 均为此提供了四个属性：innerWidth、innerHeight、outerWidth 和 innerHeight。

- IE9+、Safari 和 Firefox 中，outerWidth 和 outerHeight 返回浏览器窗口本身的尺寸（无论是从最外层的 window 对象还是从某个框架访问）。
- Opera 中这两个属性的值表示页面视图容器的大小，页面视图容器值的是 Opera 中单个标签页对应的浏览器窗口。
- Chrome 中 inner/outer Width/Height 都返回相同的值，即视口大小（viewport）大小而非浏览器窗口大小。

**注：在最新版Chrome中，innerHeight返回视口高度，而 outerHeight 返回浏览器高度；innerWidth与outerWidth默认相等（在浏览器宽高改变后，相差14-15像素值）。**

document.documentElement.clientWidth 和 document.documentElement.clientHight 中保存了页面视口的信息。不同浏览器在非标准模式下（混杂模式），会返回不同的结果。虽然无法最终确定浏览器窗口本身的大小，但却可以取得页面视口的大小，代码如下：

```javascript
var pageWidth = innerWidth,
    pageHeight = innerHeight;

if(typeof pageWidth != 'number') {
    // 判断是否为标准模式
    if(document.compatMode === 'CSS1Compat') {
        pageWidth = document.documentElement.clientWidth;
        pagHeight = document.documentElement.clientHeight;
    } else {
        pageWidth = document.body.clientWidth; // IE6 混杂模式
        pagHeight = document.body.clientHeight; // IE6 混杂模式
    }
}
```

**注：标准模式含有 '&lt;!DOCTYPE html&gt;' 头部，混杂模式不含。**

对于移动设备，有很多非常规的情形，也有各种的建议。移动开发咨询师 Peter-Paul Koch 记述了他对这个问题的研究：[移动 web 开发，推荐阅读该篇文章](http://t.cn/zOZs0Tz)；

window.resizeTo/By()：调整窗口大小，部分浏览器默认禁用该方法。

## 8.1.5 导航和打开窗口

使用 window.open() 方法既可以导航到一个特定的 URL，也可以打开一个新的浏览器窗口。他接收4个参数：要加载的 URL，窗口目标，一个特性字符串以及一个表示新页面是否取代浏览器记录中当前加载页面的布尔值。通常只需传递第一个参数。
```javascript
// 等同于 <a href="http://www.wrox.com" target="topFrame"></a>
window.open('http://www.wrox.com', 'topFrame');
```
这段代码会打开一个名为 topFrame 的窗口或框架。第二个参数还有其他几个值：_self、_parent、_top、_blank。第三个参数是一个以逗号分隔的设置字符串。

## 8.1.6 间歇调用和超时调用

- 超时调用：设置多少秒后执行，只执行一次。在个别浏览器环境下，尤其是移动端，会存在setTimeout执行完后不被回收销毁，而在持续执行状态。

```javascript
var timeoutId = setTimeout(function () {
    console.log('hello world!');
}, 1000);

clearTimeout(timeoutId);
```
- 间歇调用：设置多少秒后执行，循环执行。使用它时，会出现第二次执行会在第一次未结束就执行的情况，因而推荐使用超时调用模拟间歇调用。

```javascript
var intervalId = setInterval(function () {
    console.log('hello world!');
}, 1000);

clearInterval(timeoutId);
```
```javascript
var num = 0,
    max = 100;
var intervalId = null,
    initTimeout = null;

function intervalNumber(num, max) {
    num++;

    if(num >= max) {
        console.log('done');
        clearTimeout(intervalId);
    } else {
        clearTimeout(initTimeout);
        intervalId = setTimeout(intervalNumber, 1000);
    }
}

initTimeout = setTimeout(intervalNumber, 1000);
```

## 8.1.7 系统对话框

```javascript
alert('hello world!');

// 确认输入框
if(confirm('Are you sure?')) {
    console.log('sure');
} else {
    console.log(not sure);
}

// 带输入框的确认输入框
var result = prompt('What is your name?', '');
if(result != null) {
    console.log('welcome, ' + result);
}
```


