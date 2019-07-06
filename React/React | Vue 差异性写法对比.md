# React / Vue 差异性写法对比

****

#### 声明一些变量

* React

```js
    constructor(props){
        super(props)
        this.state ={
            num:0,
        }
    };
```

* Vue

```js
    data() {
    	return {
    		title: 'media-list',
        }
    }
```

#### 事件

* React

```html
<div onClick={this.getMsgCode.bind(this)}></div>
```

* Vue

```html
<div @click="toggle()"> </div> //点击事件
```

#### Style

* React

```html
<div style={this.state.bool ? { display: 'block' } : { display: 'none' }} >
```

* Vue

```html
<span :style="{'display':idShow ? 'block':'none'}" >搜索</span>  //注意冒号的位置
```

#### class

* React

```html
<div className={this.state.bool ? 'show' : 'hide'}></div>

<div className={`account-form-error-popup-body {this.state.errInfo ? "error-popup-in" : "error-popup-out"`}></div>  //动态添加class名
```

* Vue

```html
<div :class="{'loadingShow':isLoading}"></div>

<div class ="wrapper" :class="{'active':isOk}"></div> //动态添加class名
```

### 按条件渲染

* React


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

* Vue

```js

/* 不要同时使用v-for和v-if */
/* 用computed属性替代 */
<div class="content" v-for="(item,index) in filter"  :key="index"  v-text="item.content"></div>

data(){
	return{
		list:[{
			    title:'手机',

			},
			{
					content:'apple'
			},{
					content:'galaxy'
			},{
					company:'apple'
			}]
        }
        	computed:{
			filter:function(){
				let newlist = []
				this.list.forEach((item,index)=>{
					if(index>0&&index<3){
				 newlist.push(item)
			}
		})
	return newlist
}
	}

```