# CSS 常用语法总结

****

## 盒子居中

### 1. margin 固定宽高居中

```
#container {
    width: 600px;
    height: 500px;
    border: 1px solid #000;
    margin: auto;
}
#box {
    width: 200px;
    height: 200px;
    margin: 150px 200px;
    background-color: #0ff;
}
```

### 2. 子绝父相，负 margin 实现盒子居中

```
#container {
    position: relative;
    width: 600px;
    height: 500px;
    border: 1px solid #000;
    margin: auto;
}
#box {
    position: absolute;
    width: 200px;
    height: 200px;
    left: 50%;
    top: 50%;
    margin: -100px -100px;
    background-color: #0ff;
}
```

### 3. CSS 3 新增属性 tabal-cell

```
#container {
    display: table-cell;
    width: 600px;
    height: 500px;
    vertical-align: middle;
    border: 1px solid #000;
}
#box {
    width: 200px;
    height: 200px;
    margin: 0 auto;
    background-color: #0ff;
}
```

### 4. flex

><font color="orange">`!NOTICE`</font>IE9 以及 IE9 一下不兼容。

```
#container {
    display: -webkit-flex;
    display: flex;
    -webkit-align-items: center;
            align-items: center;
    -webkit-justify-content: center;
            justify-content: center;
    width: 600px;
    height: 500px;
    border: 1px solid #000;
    margin: auto;
}
#box {
    width: 200px;
    height: 200px;
    background-color: #0ff;
}
```

### 5. transform 居中

> <font color="orange">`!NOTICE`</font>缺点是 IE9 以下不兼容

```
#container {
    position: relative;
    width: 600px;
    height: 600px;
    border: 1px solid #000;
    margin: auto;
}
#box {
    position: relative;
    top: 50%;
    left: 50%;
    width: 200px;
    height: 200px;
    transform: translate(-50%, -50%);
    -webkit-transform: translate(-50%, -50%);
    -ms-transform: translate(-50%, -50%);
    -moz-transform: translate(-50%, -50%);
    background-color: #0ff;
}
```

## iconfont

* 第一种写法

```
@font-face{
    font-family:"icon";
    src:url(../images/icon/iconfont.ttf);
}

i {
    font-family: "icon";
    display: inline-block;
}

<i>&#xe729;</i>

```

## 清除浮动

```
.clearfix:before,
.clearfix:after {
    /*清楚浮动*/
    content: "";
    display: table;
}

.clearfix:after {
    clear: both;
}

.clearfix {
    *zoom: 1;
    /*IE/7/6*/
}
```

## focus

> 选择获得焦点的输入字段，并设置其样式：

```
input:focus
{
background-color:yellow;
}
```

## 改变 placeholder 的样式

> <font color="orange">`!NOTICE`</font> 注意兼容性

```
.input::-webkit-input-placeholder {
    color: red;
}
.input:-moz-placeholder {
    color: red;
}
.input:-ms-input-placeholder {
    color: red;
}
```

## 文本溢出显示省略号

> <font color="orange">`!NOTICE`</font>注意兼容性

* 单行文本

```
.test{
    text-overflow:ellipsis; /*设置超出部分显示...*/
    overflow:hidden;
    white-space:nowrap;/*文本不换行*/
    width:150px;}
```

* 多行文本

```
display: -webkit-box;
-webkit-box-orient: vertical;
-webkit-line-clamp: 3;
overflow: hidden;
```

* JS 实现字数限制，超出部分显示为省略号

```
if(Ext.getCmp("id").text.length >7){
  Ext.getCmp("id").setText(Ext.getCmp("id").text.substring(0,5)+"...");
}
```
