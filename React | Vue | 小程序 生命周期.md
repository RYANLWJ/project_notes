# React / Vue / Mini App 生命周期
****

### Vue 


1. `beforeCreated() `

2. `created() //在实例创建完成后被立即调用。`

3. `beforeMount()`

4. `mounted()`

5. `beforeUpdate()`

6. `updated()//当这个钩子被调用时，组件 DOM 已经更新，所以你现在可以执行依赖于 DOM 的操作。 | 最好少用,通常最好使用计算属性或 watcher 取而代之。`

7. `beforeDestroyed()`

8. `destroyed ()`


* 不常用到的生命周期钩子

`activated()`

`deactivated()`

`errorCaptured()`




### React



* <mark/>Initialization 初始阶段

`setup props and state`

* Mounting 挂载阶段

`componentWillMount()`

`render()`

`componentDidMount() //组件挂载到DOM后调用，且只会被调用一次`

* <mark/> Updation 更新阶段 
   *  React组件更新机制:setState引起的state更新或父组件重新render引起的props更新，更新后的state和props相对之前无论是否有变化，都将引起子组件的重新render。 

//props

`componentWillReceiveProps()`

`shouldComponentUpdate()  //作优化用,用得不多`

`componentWillUpdate() //很少用`

`render()`

`componentDidUpdate()//此方法在组件更新后被调用，可以操作组件更新的DOM`

//state

`shouldComponentUpdate()`

`componentWillUpdate()`

`render()`

`componentDidUpdate()`

* <mark/>Unmounting 卸载阶段 

`componentWillUnmount() //此方法在组件被卸载前调用，可以在这里执行一些清理工作，比如**清除组件中使用的定时器**，清楚componentDidMount中手动创建的DOM元素等，以避免引起内存泄漏。`




### Mini App



* 页面生命周期 

`onLoad() //监听页面加载`

`onShow()//监听页面显示`

`onReady()//监听页面初次渲染完成`

`onHide()//监听页面隐藏`

`onUnload()//监听页面卸载`


