# Ajax

****

## Ajax 请求的封装 以使用 axios 为例

    * 创建一个 api 文件夹
      * 在 api 文件夹里创建 index.js, 和 gobalAjax 这两个文件

#### index.js

```js
import gobalAjax from './gobalAjax'
// const BASE = 'http://localhost:5000'
const BASE = ''
export const checkNumber = (data) => gobalAjax(BASE + '/api/user/checkNum', { data}, 'POST')

```

#### gobalAjax.js

```js

import axios from 'axios';
//promise / then
export default function ajax(url, data={}, type = 'GET') {
    let keyName= Object.keys(data)
       data = data[keyName[0]]
       console.log(data)
    return new Promise((resolve, reject) => {
        let promise
        // 1. 执行异步ajax请求
        if (type === 'GET') { // 发GET请求
            promise = axios.get(url, { // 配置对象
                params: data // 指定请求参数
            })
        } else { // 发POST请求
            promise = axios.post(url, data)
        }
        // 2. 如果成功了, 调用resolve(value)
        promise.then(response => {
            resolve(response)
            // 3. 如果失败了, 不调用reject(reason), 而是提示异常信息
        }).catch(error => {
            // reject(error)
            message.error('请求出错了: ' + error.message)
        })
    })


}
// 使用 async / await
export default async function ajax(url, data = {}, type = 'GET') {
    let keyName = Object.keys(data)
    data = data[keyName[0]]
    console.log(data)
    let makeRequest

    if (type === 'GET') {
        makeRequest = async () => {
            const res = await axios.get(url, data).catch((error) => console.log(error))
            return res;
        }
    } else {
        makeRequest = async () => {
            const res = await axios.post(url, data).catch((error) => console.log(error))
            return res;
        }
    }
    return await makeRequest()

}
```
