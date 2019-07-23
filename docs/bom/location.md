# 8.2 location 对象

location 是最有用的 BOM 对象之一，它提供了与当前窗口中加载的文档有关的信息，还提供了一些导航功能。它既是window对象的属性，同时也是 document 对象的属性。它们俩引用的是同一个对象。

下表为 location 对象的所有属性：

| 属性名   | 例子                  | 说明                                                                      |
|----------|-----------------------|---------------------------------------------------------------------------|
| hash     | "#contents"           | 返回URL中的hash（#号后跟零或多个字符），如果URL中不包含散列，则返回空字符 |
| host     | "www.wrox.com:80"     | 返回服务器名称和端口号（如果有）                                          |
| hostname | "www.wrox.com"        | 返回不带端口号的服务器名称                                                |
| href     | "http://www.wrox.com" | 返回当前加载页面的完整 URL。而location对象的toString()方法也返回这个值    |
| pathname | "/wileyCDA/"          | 返回URL中的目录和（或）文件名                                             |
| port     | "8080"                | 返回URL中指定的端口号，如果URL中不包含端口号，则返回空字符串              |
| portocol | "http:"               | 返回页面使用的协议。通常是http或https                                     |
| search   | "?q=javascript"       | 返回URL的查询字符串，这个字符串以问号开头                                 |

## 8.2.1 查询字符串参数

解析查询字符串，返回包含参数的一个对象：

```javascript
function getQueryStringArgs () {
    // 取得查询字符串并去掉开头的问号
    var qs    = (location.search.length > 0 ?
                location.search.substring(1) :
                ""),
        // 保存数据的对象
        args  = {},
        // 取得每一项
        items = qs.length ? qs.split('&') : [],
        item  = null,
        name  = null,
        value = null,
        // 在for循环中使用
        i     = 0,
        len   = items.length;

    for(i = 0; i < len; i++) {
        item  = items[i].split('=');
        name  = decodeURIComponent(item[0]);
        value = decodeURIComponent(item[1]);

        if(name.length) {
            args[name] = value;
        }
    }

    return args;
}
getQueryStringArgs();
```

## 8.2.2 位置操作

使用 location 对象可以通过很多方式来改变浏览器的位置。

```javascript
window.assign('www.annavy.info');
```

下面两行代码，显示调用 window.assign();

```javascript
window.location = 'www.annavy.info';
location.href = 'www.annavy.info';
```

另外，修改location对象的其他属性也可以改变当前加载的页面。

```javascript
// 假设初始 URL 为 http://www.annavy.info/javascript/

// 将 URL 修改为 http://www.annavy.info/javascript/#section1
location.hash = '#section1';

// 将 URL 修改为 http://www.annavy.info/javascript/?q=js
location.search = '?q=js';

// 将 URL 修改为 http://www.wrox.com/javascript/
location.hostname = 'www.wrox.com';

// 将 URL 修改为 http://www.annavy.info/vue/
location.pathname = 'vue';

// 将 URL 修改为 http://www.annavy.info:8080/javascript/
location.port = '8080';
```

每次修改（hash 除外），页面都会以新URL重载。

**注：在 IE8、Firefox 1、Safari 2+、Opera 9+、Chrome中，修改hash的值会在浏览器的历史记录中生成一条新记录。**

- replace：禁用后退按钮，同时也不会再历史记录中生成新纪录。在调用replace()之后，用户不能回到前一个页面。

```javascript
location.replace('www.wrox.com');
```

- reload：重新加载当前页面，接收一个参数 true/false。

```javascript
location.reload(); // 有可能从缓存中重新加载页面
location.reload(true); // 将从服务器中重新加载
```