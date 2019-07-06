# Vue 结合 express 连接 mongodb

## 目录

- [在项目根目录创建一个server文件夹](#在项目根目录创建一个server文件夹)
   - [app.js](#appjs)
   - [db.js](#dbjs)
   - [user.js](#userjs)
   - [api.js](#apijs)
- [Vue](#vue)
- [Mongodb在终端常用指令](#Mongodb在终端常用指令)
****

## 在项目根目录创建一个 server 文件夹

### 里面包含：

1. api.js 创建后端接口 （一些查询语句也可以写在里面）
2. app.js 创建 express 服务器
3. db.js 连接 mongodb 数据库
4. user.js 配置 Schema 模型，用来连接表和操作表

### app.js

```js
// 引入express模块
const express = require('express');

// 创建app对象
const app = express();

const query = require('./api.js');

const bodyParser = require("body-parser")

app.use(bodyParser.json());

app.use(bodyParser.urlencoded({ extended: false }));

// 使用
app.use('/api', query);

// 定义服务器启动端口
app.listen(3009, () => {
  console.log('app listening on port 3009');
});
```

### db.js

```js
var mongoose = require('mongoose');
var DB_URL = 'mongodb://localhost:27017/taobaodb';//后缀是数据库的名字


/* 链接 */
mongoose.connect(DB_URL,{ useNewUrlParser: true });

/* 链接成功 */
mongoose.connection.on('connected', function() {
  console.log('Mongoose connection open to ' + DB_URL);
});

// 链接异常
mongoose.connection.on('error', function(err) {
  console.log('Mongoose connection error:' + err);
});

// 链接断开

mongoose.connection.on('disconnected', function() {
  console.log('Mongoose connection disconnected');
});
module.exports = mongoose;

```

### user.js

```js
/*
*定义一个user的Schema
*/
const mongoose = require('./db.js');
const Schema = mongoose.Schema;

/*创建一个模型
*定义一些数据
*/

const UserSchema = new Schema({
  name: { type: String }, // 属性name，类型为String
  age: { type: Number, default: 30 }, // 属性age，类型为Number，默认值为30
  sex: { type: String },
  location:{type:Array},
});

// 导出model模块
const User = module.exports = mongoose.model('taobaodb', UserSchema,'taobaodb');
/* ! 这里的第三个参数才是表名 */
```

### api.js

```js
// 引入express 模块
const express = require('express');

const router = express.Router();

// 引入user.js
const User = require('./user');

// 新增一条数据 接口为add
router.post('/add', (req, res) => {
  const user = new User({
    name: req.body.name,
    age: req.body.age,
    sex: req.body.sex
  });
  user.save((err, docs) => {
    if (err) {
      res.send({ 'code': 1, 'errorMsg': '新增失败' });
    } else {
      res.send({ "code": 0, 'message': '新增成功' });
    }
  });
});

// 查询数据
router.post('/query', (req, res) => {
  const name = req.body.name,
    age = req.body.age,
    sex = req.body.sex;
  const obj = {};
  if (name !== '') {
    obj['name'] = name;
  }
  if (age !== '') {
    obj['age'] = age;
  }
  if (sex !== '') {
    obj['sex'] = sex;
  }
  User.find(obj, (err, docs) => {
    if (err) {
      res.send({ 'code': 1, 'errorMsg': '查询失败' });
    } else {
      res.send(docs);
    }
  });
});

// 测试插入数据
var user = new User({
    name: 'RYAN',
    sex: 'male',
    age: 26,
    location:[{
        name:'cherry',
        age:33
    },
    {
        name:'cherry',
        age:33
    }]

  });

  user.save(function(err, res) {
    if (err) {
      console.log(err);
    } else {
      console.log(res);
    }
  });




/* --------------------------------------------------------------- */
// 根据 _id 删除数据
router.post('/del', (req, res) => {
  const id = req.body.id;
  // 根据自动分配的 _id 进行删除
  const whereid = { '_id': id };
  User.remove(whereid, (err, docs) => {
    if (err) {
      res.send({ 'code': 1, 'errorMsg': '删除失败' });
    } else {
      res.send(docs);
    }
  })
});

// 更新数据
router.post('/update', (req, res) => {
  console.log(req.body)
  // 需要更新的数据
  const id = req.body.id,
    name = req.body.name,
    age = req.body.age,
    sex = req.body.sex;
  const updateStr = {
    name: name,
    age: age,
    sex: sex
  };
  const ids = {
    _id: id
  };
  console.log(ids);
  User.findByIdAndUpdate(ids, updateStr, (err, docs) => {
    if (err) {
      res.send({ 'code': 1, 'errorMsg': '更新失败' });
    } else {
      res.send(docs);
    }
  });
});
module.exports = router;
```

## Vue

#### TEST demo

```js
    <a class="btn buy-btn" @click="addData()" >加入购物车</a>
.
..
...
methods:{
addData(){

        let data = {
         name:'RYAN',
         age:26,
         sex:'male',
        }
        this.$http.post('/api/add', {
          data
        }).then(
        res => console.log(res) //输出反馈信息
        )}
.
..
...
/* [OPTIONS]使用axios的写法 */
    addData(){
        let data = {
         name:'RYAN',
         age:26,
         sex:'male',
        }
        this.$axios.post('/api/add', {
         data
        }).then(
          res=>console.log(res)
          )


    }
```

## Mongodb 在终端常用指令

1. 开启
    * 找到 mongodb 这个文件夹然后在 bin 打开终端，输入

```
  $ sudo ./mongod
```

2. 正常关闭
   * 在 bin 文件夹里点开 mongo 这个终端窗口，输入

```
  > use admin;
  switched to db admin
  > db.shutdownServer();
```
