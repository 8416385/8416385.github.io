---
layout: post
title: JS-xlsx到处excel表格
subtitle: 数据导出为excel文件
date: 2020-12-11
author: listen_spacer
header-img: img/background1.jpg
catalog: true
tags:
  - JavaScript
  - 文件导出
---

有时我们会有将 table 或者列表数据导出 excel 表格的需求，如果你也碰到了这种需求。
那么可以使用下面这个方法。

**话不多说，直接开整。**

##### 1、获取列头数据

这里分为两种情况：
1、单行表头
2、复杂表头

单行表头比较简单这里就简单跳过了，直接说负责表头。
先创建一个 excel，里面只存放复杂表头。然后通过下面代码到处列头数据。

```
<!DOCTYPE html><html><head>
<meta charset="UTF-8">
<title></title>
<script src="xlsx.core.min.js"></script></head><body>
    <input type="file" onchange="importf(this)" />
    <div id="demo"></div>
<script>
    var WB;
    var rABS = true; //是否将文件读取为二进制字符串
    function importf(obj) {//导入
        if (!obj.files) { return; }
        var f = obj.files[0];
        {
            var reader = new FileReader();
            var name = f.name;
            reader.onload = function (e) {
            var data = e.target.result;
            WB = XLSX.read(data, { type: 'binary' });
            document.getElementById("demo").innerHTML = JSON.stringify(WB.Sheets[WB.SheetNames[0]]);
        };
        if (rABS) reader.readAsBinaryString(f);
        else reader.readAsArrayBuffer(f);
    }
}
</script></body></html>
```
导出列头数据之后，将数据保存并尝试导出一个excel表格

##### 2、尝试导出列头
```
<!DOCTYPE html><html><head>
<meta charset="UTF-8">
<title></title>
<script src="xlsx.core.min.js"></script>
<script src="xlsx.utils.min.js"></script>
<script>
function saveAs(obj, fileName) {//当然可以自定义简单的下载文件实现方式
var tmpa = document.createElement("a");
tmpa.download = fileName || "下载";
tmpa.href = URL.createObjectURL(obj); //绑定a标签
tmpa.click(); //模拟点击实现下载
setTimeout(function () { //延时释放
URL.revokeObjectURL(obj); //用URL.revokeObjectURL()来释放这个object URL
}, 100);
}
</script></head>

<body>
<input type="button" onclick="downloadExl()" value="导出" />
<div id="demo"></div>
<script>
var head = 列头数据;
function downloadExl() {
var wb = xlsxUtils.format2WB(head, undefined, undefined, "A1:F3");
saveAs(xlsxUtils.format2Blob(wb), "这里是下载的文件名.xlsx");
}</script></body></html>
```

###### 3、导出现有数据
以vue代码为例。
```
export default {
    data:{
        keyMap:[列1，列2，列3]  //通过设置数组让导出时可以按顺序显示，值对应到处数据对象中的key
    },
    methods:{
         downloadExl: function () {
            var self = this;
            // 导出的数据的格式是Array<Object>
            var data = xlsxUtils.format2Sheet(导出的数据, 0, 6, self.keyMap); //偏移6行按keyMap顺序转换，偏移行数和表头所占行数相等
            var dataKeys = Object.keys(data);
            var head = 列头数据;
            for (var k in head) data[k] = head[k]; //追加列头
            var wb = xlsxUtils.format2WB(data, undefined, undefined, "A1:" + dataKeys[dataKeys.length - 1]);
            saveAs(xlsxUtils.format2Blob(wb), "年度R&D经费报表.xlsx");
        },
        // 自定义简单的下载文件实现方式，但是不支持IE
        // 如果想要支持ie下载，可以引入FileSaver.js 使用这个插件的保存方法就可以了
        saveAs: function (obj, fileName) { 
            var tmpa = document.createElement("a");
            tmpa.download = fileName || "下载";
            tmpa.href = URL.createObjectURL(obj); //绑定a标签
            tmpa.click(); //模拟点击实现下载
            setTimeout(function () { //延时释放
                URL.revokeObjectURL(obj); //用URL.revokeObjectURL()来释放这个object URL
            }, 100);
        },
    }
}
```