# Typescript 语法导致的常见报错

#### 1. 在main.ts无法引进自定义模块的global.js文件
  * 解决方案
    1. 在main.ts使用require引进自定义模组
    2. 修改项目根目录的tsconfig.json文件
```
"noImplicitAny": false,//让ts不报parameter implicitly has an 'any' type的错误
```