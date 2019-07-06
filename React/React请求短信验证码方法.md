# 请求短信验证码 API 配置 (Node)

## 配置 demo:

### 需要引进的依赖：

1. **request**
2. **querystring**

#### main.ts

```js
    const request = require('request'); // 发送短信 验证码用到的module
    const querystring = require('querystring'); //  发送短信验证码用到的module

    /*把request和querystring挂到Vue,供其他模块使用*/
    Vue.prototype.$request = request;
    Vue.prototype.$querystring = querystring;
```

#### signIn.vue

```js
/*未设置变量*/
  methods: {
    getMsgCode(){
      var queryData = this.$querystring.stringify({
         "mobile": "18666086022",  // 接受短信的用户手机号码
         "tpl_id": "155597",  // 您申请的短信模板ID，根据实际情况修改
         "tpl_value": "#code#=520520",  // 您设置的模板变量，根据实际情况修改
         "key": "c0570ef0cf102c596043aee5b9bddb16",  // 应用APPKEY(应用详细页查询)
      });
      var queryUrl = 'http://v.juhe.cn/sms/send?'+queryData;
      this.$request(queryUrl, function (error, response, body) {
      	if (!error && response.statusCode == 200) {
      		console.log(body) // 打印接口返回内容
      		var jsonObj = JSON.parse(body); // 解析接口返回的JSON内容
      		console.log(jsonObj)
      	} else {
          console.log('请求异常');
          console.log(error)
      	}
       })

    }
  },
```
