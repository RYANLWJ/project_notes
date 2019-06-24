# React（豆瓣）项目总结

****

## 注册页

>**业务逻辑**：在数据库查找有无当前用户的数据，有的话直接登录，没有的话就直接注册。<br/>
>**应用到的技术（方法）**: 正则判断，倒计时，生成随机数，短信请求，request,js-cookie,querystring, 跨域。<br/>
>**待解决问题**: 短信请求的返回信息跨不了域。

### 获取验证码倒计时

```
let count = this.state.count;
       const timer = setInterval(() => {
           this.setState({ count: (count--), ready: false }, () => {
               if (count === 0) {
                   clearInterval(timer);
                   this.setState({
                       ready: true,//恢复点击有用
                       count: 60
                   })
               }
           });
       }, 1000);
```

### setState() 的回调使用

```
    handelNumVal(e) {
        this.setState({
            userNum: e.target.value

        }, () => this.inputConfirm())
        //setState是异步的,如果要拿到最后的值,要用回调函数

    };
```

### 生成四位随机数

```
rand(min, max) {
    return Math.floor(Math.random() *(max - min)) + min;
}
```

### 动态添加样式

```
className={`account-form-error-popup-body ${this.state.errInfo ? "error-popup-in" : "error-popup-out"}`}
```

## 主页

>**业务逻辑**：按数据规则实现数据渲染。<br/>
>**解决方案**：面对不规则的数据块，可以利用在 map 方法里面添加判断进行相应的数据渲染。<br/>
>**应用到的技术（方法）**:map(),axios

### 解决在线图片无法加载

```
  <meta name="referrer" content="no-referrer" />
  <!-- 在index.html这个可以加载在线受限图片 -->
```

### 解决无法引入样式
* 在项目根目录找到.webpackrc文件添加以下内容
```
{
    "disableCSSModules": true
}

```
## 跨域

### 解决方法：

1. 在。roadhogrc.mock.js 文件添加一下语句

```
export default{
    'POST /api/*': 'http://localhost:3336',
}
```
