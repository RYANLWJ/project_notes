# Vue 项目问题汇总
- [异步](#异步)   
- [功能](#功能)        
  - [连接本地服务器](#连接本地服务器)        
     -  [db.js](#dbjs)        
     -  [index.js](#indexjs)        
     -  [userApi.js(用户接口,sqlMap里面的语句)](#userapijs用户接口sqlmap里面的语句)   -  [sqlMap.js](#sqlmapjs)        
     -  [detail.vue](#detailvue)        
     -  [main.js](#mainjs)  
  -  [返回上一页](#返回上一页)    
  -  [router.js (列表切详情的配置)](#routerjs-列表切详情的配置)    
  -  [把商品id传到详情页](#把商品id传到详情页)    
  -  [获取从列表页传过来的路由id](#获取从列表页传过来的路由id)    
  -  [监听路由变化](#监听路由变化)    
  -  [回调的使用](#回调的使用)
  - [编程式跳转](#编程式跳转)
  -  [Cookie](#Cookie)
- [语法](#语法)
- [逻辑思路](#思路)


****

### 异步
1. 如果遇到拿到的数据有多层嵌套的(如数组中套对象)数据,要进行二次循环的时候,可能需要解决异步导致拿不到相应数据的问题.

```html
  <img
      v-if="list2.category_list[0].body.items"
      style="height: 2rem; width: 5rem;"
      data-src="//i8.mifile.cn/v1/a1/   c727b13a-cab6-70c9-5c94-79a30c5aee51!500x200.webp"
      :key="idx2"
      :src="list2.category_list[0].body.items[0].img_url"
  >
```

2. setTimeout
   
```js
  mounted() {
      setTimeout(() => {
           this.initPage()
      },20 );
        },
```
### 功能

### 连接本地服务器

1. #### 配置跨域可能会导致的问题

```js
module.exports = {
  /** 区分打包环境与开发环境
  * process.env.NODE_ENV==='production'  (打包环境)
  * process.env.NODE_ENV==='development' (开发环境)
  * baseUrl: process.env.NODE_ENV==='production'?"https://cdn.didabisai.com/  front/":'front/',
  */

  // 项目部署的基础路径

  // 我们默认假设你的应用将会部署在域名的根部,

  // 例如 https://www.my-app.com/

  // 如果你的应用部署在一个子路径下，那么你需要在这里

  // 指定子路径。比如将你的应用部署在

  // https://www.foobar.com/my-app/

  // 那么将这个值改为 '/my-app/'

  baseUrl: "/", // 构建好的文件输出到哪里

  outputDir: "dist", // where to put static assets (js/css/img/font/...) // 是  否在保存时使用‘eslint-loader’进行检查 // 有效值: true | false | 'error' // 当设置为 ‘error’时，检查出的错误会触发编译失败

  lintOnSave: true, // 使用带有浏览器内编译器的完整构建版本 // https://vuejs.org/v2/  guide/installation.html#Runtime-Compiler-vs-Runtime-only

  runtimeCompiler: false, // babel-loader默认会跳过`node_modules`依赖. // 通过这个  选项可以显示转译一个依赖

  transpileDependencies: [
  /* string or regex */
  ], // 是否为生产环境构建生成sourceMap?

  productionSourceMap: false, // 调整内部的webpack配置. // see https:// github.com/vuejs/vue-cli/blob/dev/docs/webpack.md

  chainWebpack: () => {},

  configureWebpack: () => {}, // CSS 相关选项

  css: {
  // 将组件内部的css提取到一个单独的css文件（只用在生产环境）

  // 也可以是传递给 extract-text-webpack-plugin 的选项对象

  extract: true, // 允许生成 CSS source maps?

  sourceMap: false, // pass custom options to pre-processor loaders. e.g. to  pass options to // sass-loader, use { sass: { ... } }

  loaderOptions: {}, // Enable CSS modules for all css / pre-processor files. / / This option does not affect *.vue files.

  modules: false
  }, // use thread-loader for babel & TS in production build // enabled by  default if the machine has more than 1 cores

  parallel: require("os").cpus().length > 1, // PWA 插件相关配置 // see https://  github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-pwa

  pwa: {}, // configure webpack-dev-server behavior

  devServer: {
  open: process.platform === "darwin",

  disableHostCheck: false,

  host: "0.0.0.0",

  port: 8088,

  https: false,

  hotOnly: false, // See https://github.com/vuejs/vue-cli/blob/dev/docs/  cli-service.md#configuring-proxy

  proxy:{
  '/api': {
  target: 'http://localhost:3336/api/',
  //3336为服务器端口,前面是本机ip地址,每次改了端口都要记得重启vue
  changeOrigin: true,
  pathRewrite: {
  '^/api': ''
  }
  }
  }

  // before: app => {}
  }, // 第三方插件配置

  pluginOptions: {
  // ...
  }

  };
```
2. #### 项目根目录创建一个server文件夹,里面包含(db.js/index.js/sqlMap/userApi这几个文件)

### db.js

```js
const express = require('express');
const mysql = require('mysql');
const app = express();

module.exports = {

        mysql: {
        
        host: 'localhost',

        user: 'root',

        password: 'root',

        database: 'goodslist',

        port: 8889,

        multipleStatements: true

        }

        }  
```

### index.js

```js
// node 后端服务器

  const userApi = require('./userApi');

  const fs = require('fs');//文件管理系统

  const path = require('path');//path 模块提供用于处理文件路径和目录路径的实用工具。

  const bodyParser = require('body-parser');//处理程序之前，在中间件中对传入的请求体进行解析

  const express = require('express');

  const app = express();

  app.use(bodyParser.json());

  app.use(bodyParser.urlencoded({extended: false}));

  const {PORT} =require('./config.json');//获取配置文件

  app.use(express.static('./'));
  // 后端api路由

  app.use('/api/user', userApi);

  // 监听端口

  app.listen(PORT);

  console.log('success listen at port '+ PORT);

  // 记得每次修改了都要启动一下,或使用supervisor

```


### userApi.js(用户接口,sqlMap里面的语句)

```js
  var models = require('./db.js');

  var express = require('express');

  var router = express.Router();

  var mysql = require('mysql');

  var $sql = require('./sqlMap.js');

  // 连接数据库

  var conn = mysql.createConnection(models.mysql);

  conn.connect();

  var jsonWrite = function(res, ret) {

  if(typeof ret === 'undefined') {

  res.json({

  code: '1',

  msg: '操作失败'

  });
  console.log('fail')

  } else {

  res.json(ret);
  console.log('ok')

  }

  };

// 增加用户接口

  router.post('/addUser', (req, res) => {

  var sql = $sql.user.add;

  var params = req.body;

  console.log(params);

  conn.query(sql, [params.username, params.age], function(err, result) {

  if (err) {

  console.log(err);

  }

  if (result) {

  jsonWrite(res, result);

  }

  })

  });
// 查询数据

  router.post('/ser', (req, res) => {

  var sql = $sql.user.ser;

  var params = req.body;

  console.log(params);

  conn.query(sql, [params.username, params.age], function(err, result) {

  if (err) {

  console.log(err);

  }

  if (result) {
  console.log(result)
  jsonWrite(res, result);

  }

  })

  });

module.exports = router;

```

### sqlMap.js

```js
// sql语句
var sqlMap = {
// 用户
user: {
add: 'insert into user(id, name, age) values (0, ?, ?)'
}
}

module.exports = sqlMap;
```
### detail.vue

```  
  //加入购物车
  dropToCart(goodsId){
  console.log(goodsId)
  var name = '21122';

  var age = 2112;

  this.$http.post('/api/user/addUser', {
  //后端接口路径
  username: name,

  age: age

  },{}).then((response) => {

  console.log(response);

  })

  }
```

### main.js

```js
  import VueResource from 'vue-resource';
  import VueRouter from 'vue-router';
  Vue.use(VueResource)
  Vue.use(VueRouter)
```



## 返回上一页

```js
  back(){
  this.$router.go(-1);//返回上一层
  }
```
## router.js (列表切详情的配置)

```js
  {
    path: '/detail/:id',
    name: 'detail',
    component: Detail,

  }
```
## 把商品id传到详情页

```js
  goToDetail(id){
  this.$router.push({
  path:'detail/'+id
  })
  }
```
## 获取从列表页传过来的路由id

```js
  getOrderId(){
  this.id=this.$route.params && this.$route.params.id
  console.log(this.id)
  //获取列表页传过来的id
  }
``` 
## 监听路由变化

```js
  mounted() {
  this.getUrlName();
  // console.log(this.$route.params )
  },
  watch: {//监听路由变化
  '$route':'getUrlName'
  },
  methods: {
  active(currIdx) {
  getUrlName(){
  this.url_name=this.$route.path.slice(1);//截取掉 "/"
  if(!this.url_name){ //如果url路径为空,则显示首页按钮高亮
  this.url_name='home';
  }
  console.log(this.url_name)
  //获取列表页传过来的id
  }
  },
```

## 回调的使用

* 应用在详情页,查询当前用户所看的当前商品是否已在此用户的订单中,如果没有则插入,有则更新.
  
```js
checkCurrItem(res){
    var name = 'RYAN';
  var goods_id = this.id;
  this.$http.post('/api/user/checkCurr', {
       //后端接口路径
      username:name,
      goodsId:goods_id
      },{}).then((response) => {
      console.log(response);

        if(response.body==false){//数据库没有这条商品id []==false? >true

         return res(true);
        }else if(response.body){
          //  this.currQty=response.body.qty
          //  console.log(this.currQty)
          return res(false);
        }
```

```js
dropToCart(){
      this.checkCurrItem(res=>{
        // console.log(res,err)
        if(res){
          console.log(res)
          var name = 'RYAN';
          var qty = 1;
          var goods_id=this.id;
          this.$http.post('/api/user/addUser', {
            
          username: name,
          quantity:qty,
          goodsId:goods_id

          },{}).then((response) => {
          console.log(response);
          this.checkQty();
         })
        };
          if(!res){
          console.log('已经有了')
          this.currQty ++
          this.updateQty() //更新数量
          this.checkQty()
        }
    
      })
    
  }
```

## 编程式跳转

```js
  this.$router.push({
          path:'/signin'
        })
```
## Cookie
* 方法:
  1. 使用js-cookie
   
```
  npm i js-cookie
```
```js
  import Cookies from 'js-cookie'
```
```js
Cookies.get('name'); // => 'value'
Cookies.get('nothing'); // => undefined
Read all visible cookies:

Cookies.get(); // => { name: 'value' }
Delete cookie:

Cookies.remove('name');
Delete a cookie valid to the path of the current page:

Cookies.set('name', 'value', { path: '' });
Cookies.remove('name'); // fail!
Cookies.remove('name', { path: '' }); // removed!
IMPORTANT! when deleting a cookie, you must pass the exact same path and domain attributes that was used to set the cookie, unless you're relying on the default attributes.

Note: Removing unexisting cookie does not raise any exception nor return any value
```

2. 直接在Vue里写js操作
   
```js
set: function (name, value, days) {

    var d = new Date;

    d.setTime(d.getTime() + 24*60*60*1000*days);

    window.document.cookie = name + "=" + value + ";path=/;expires=" + d.toGMTString();

},

get: function (name) {

    var v = window.document.cookie.match('(^|;) ?' + name + '=([^;]*)(;|$)');

    return v ? v[2] : null;

},

delete: function (name) {

    this.set(name, '', -1);

}
```


# 语法

* :style

```js
  :style="{'transform':(num!=0&&12<=num)?'translateY(-10.8rem)':''}"
```
* :class

```js
  :class="{'active':idx==num}"
```  

* 判断对象是否为空

```js
 if(Object.keys(obj)==0){
    return true
    }else{
      return false
    }
```
* 判断数组是否为空

```js
  if(array.length==0){
return true
    }else{
      return false
    }
  
```

# 业务逻辑
  ## 注册页

## 商品-->详情(遮罩)
  * 需求
    > 在列表页点击点击商品,显示相应的详情内容
  * 思路
    > 可以使用传一个当前项的对象过去(使用公共仓库),然后在详情页使用computed把相应内容渲染出来

## 商品数量的增/减
  * 需求
    > - 实现用户对每项商品的增减需求
    > - 实现总价的实时显示
    > - 把商品数据传入到数据库
  * 思路
    > - 
    





