# React（豆瓣）项目总结

****

## 注册页

>**业务逻辑**：在数据库查找有无当前用户的数据，有的话直接登录，没有的话就直接注册。<br/>
>**应用到的技术（方法）**: 正则判断，倒计时，生成随机数，短信请求，request,js-cookie,querystring, 跨域。<br/>
>**待解决问题**: 短信请求的返回信息跨不了域。

### 获取验证码倒计时

```js
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

```js
    handelNumVal(e) {
        this.setState({
            userNum: e.target.value

        }, () => this.inputConfirm())
        //setState是异步的,如果要拿到最后的值,要用回调函数

    };
```

### 生成四位随机数

```js
rand(min, max) {
    return Math.floor(Math.random() *(max - min)) + min;
}
```

### 动态添加样式

```js
className={`account-form-error-popup-body ${this.state.errInfo ? "error-popup-in" : "error-popup-out"}`}
```

### 点击跳转到主页

`import {Link,withRouter} from 'dva/router';`

```js
export default withRouter( class SignIn extends React.Component { 
    
})

```

` this.props.history.push('/home')//跳转到主页`

## 主页

>**业务逻辑**：按数据规则实现数据渲染。<br/>
>**解决方案**：面对不规则的数据块，可以利用在 map 方法里面添加判断进行相应的数据渲染。<br/>
>**应用到的技术（方法）**:map(),axios, 缓存<br/>

##### 流程

* 查询浏览器是否有缓存(sessionStorage)
* 如有 -->通过拿缓存的数据渲染(render)
* 如无 -->请求服务器拿数据渲染，并把当前数据存储到缓存。(axios)
* 滚动到当前请求的内容末尾处请求加载更多（在加载进行中，禁止用户的请求触发）(scroll)

### 查询有无缓存内容

```js
    checkCache() {
        if (sessionStorage.getItem(this.state.tab)) {
            // console.log('读缓存')
            const res = JSON.parse(sessionStorage.getItem(this.state.tab))
            // console.log(res)
            this.setState({ content: res})
        } else {
            this.getContent();
        }
    };
```

### 请求数据

```js
   async  getContent() {
        let data = []
        const res = await axios.get('https://www.easy-mock.com/mock/5d0da1be896dbe7836ce5769/example/homefeed1')
        data = res.data.items
        // console.log(res)

        data = this.randomSort1(data)//把数组打乱,让内容随机出现

        sessionStorage.setItem(this.state.tab, JSON.stringify([...this.state.content, ...data]))//存缓存
        this.setState({ content: [...this.state.content, ...data], }, () => {
            this.setState({ isOk: true, })
        })
    };
```

### 渲染

```js
 {
     this.state.content.map((item, index) => {
            if (item.content.photos.length == 3) {
            return <PhotoCard content={item} key={index} />
            } else if (item.content.photos.length == 0) {
            return <NormalCard content={item} key={index} />
            } else if (item.content.photos.length == 1) {
            return <CoverCard content={item} key={index} />
        }
    })
 }

```

### 获取内容区盒子高度

```js
//js部分
   componentDidUpdate() {

        if (this.state.isOk) {
            console.log(this.refs.content_wrap.offsetHeight)

            this.setState({
                content_h: this.refs.content_wrap.offsetHeight,
                isOk: !this.state.isOk
            })
        }

    };
//html部分
   <div className="page" ref="content_wrap" >
```

### 滚动到指定位置触发加载更多

```js
  window.addEventListener('scroll', () => {
            let scrollTop = document.documentElement.scrollTop;
            // console.log(scrollTop+667)
            //667=(_this.state.content_h-scrollTop)
            if ((scrollTop + 667) % _this.state.content_h === 0 && scrollTop !== 0) {
                _this.loadMore()
            }
        })
```

### 解决在线图片无法加载

```html
  <meta name="referrer" content="no-referrer" />
  <!-- 在index.html这个可以加载在线受限图片 -->
```

### 解决无法引入样式

* 在项目根目录找到。webpackrc 文件添加以下内容

```json
{
    "disableCSSModules": true
}

```

## 跨域

### 解决方法：

1. 在。roadhogrc.mock.js 文件添加一下语句

```js
export default{
    'POST /api/*': 'http://localhost:3336',
}
```
