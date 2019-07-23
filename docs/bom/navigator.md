# 8.3 navigator 对象

识别浏览器客户端的事实标准。

## 8.3.1 检测插件

检测浏览器中是否安装了特定插件是一种常见的检测里程。对于非IE浏览器，可以使用plugins数组来达到这个目的。该数组中的每一项都包含下列属性：

- name：插件的名字；
- description：插件的描述；
- filename：插件的文件名；
- length：插件所处理的MIME类型数量。

通过循环迭代每个插件并将插件的 name 与给定的名字进行比较，一下方式适用于除 IE 外的浏览器：

```javascript
function hasPlugin(name) {
    name = name.toLowerCase();
    for (var i = 0, l = navigator.plugins.length; i < l; i++) {
        if(navigator.plugins[i].name.toLowerCase().indexOf(name) > -1) {
            return true;
        }
    }

    return false;
}

// 检测 Flash
console.log(hasPlugin('Flash'));

// QuickTime
console.log(hasPlugin('QuickTime'));
```

检测IE中的插件

```javascript
function hasIEPlugin(name) {
    try {
        new ActiveObject(name);
        return true;
    } catch(e) {
        return false
    }
}

// 检测 Flash
console.log(hasPlugin('ShockwaveFlash.ShockwaveFlash'));
```

检测所有浏览器中是否存在某个插件的方法：

```javascript
function hasPluginItem(name) {
    var result = hasPlugin(name);
    if(!result) {
        result = hasIEPlugin(name);
    }

    return result;
}
```

## 8.3.2 注册处理程序

注册方法：registerProtocolHandle()，接收三个参数，要处理的协议（例如，mailto或ftp），处理该协议的页面和应用程序的名称。

