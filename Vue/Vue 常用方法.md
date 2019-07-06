# Vue 常用方法

****

### 组件之间的通信

##### 父子通信 - props / $emit

 * 父组件

```js
    <MediaList :content="name" @sendChildData="getChildData"/>
	data() {
			return {
				title: 'Hello',
				name:'我来自父组件'
			}
		},
        methods: {
			getChildData(data){
				console.log(data)
				}
		},
```

  *  子组件

```
	<view @click="sendChildData()">
			{{content}}
		</view>
	data() {
			return {
				title: 'media-list',
				tt:'我来自子组件',
            }
    },
    props:['content'],
    methods:{
			sendChildData(){
			console.log('点击了子组件')
			this.$emit('sendChildData',this.tt)

			}
		}

```

### 按条件渲染（过滤某些不要的项）

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
