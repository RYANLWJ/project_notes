# React.js

****

## 基本语法 / 功能

1. 用 react.js 实现类似 vue.js 的 v-if / v-for 的指令

```
    const tag = <div>real content </div>;
    const tag2 = <p>false content</p>;
    const data = {
    bool: true,
    content: ['你好', '你是谁', '外国人']
    }
    const methods = {

        vIf: (bool, tag1, tag2) => {
            return bool ? tag1 : tag2
        },
        vFor: array => {
            return array.map((item, index) => {
                return <p key={index}>{item}</p>
            })
        }
    }
```

2. toggle 功能

```
    import React from 'react';//引进react

    class Header extends React.Component {   //构建组件
        constructor(props) {
            super(props)
            this.state = { isToggleOn: true }
            this.handleClick = this.handleClick.bind(this)//修正点击事件的this指向
        }

        handleClick() {
            this.setState(prevState => ({
                isToggleOn: !prevState.isToggleOn
            }))
        }

        render() {
            const tag1 = <div>我是第一条内容</div>
            const tag2 = <div>我是第二条内容</div>
            return (
                <div onClick={this.handleClick}>
                    {this.state.isToggleOn ? tag1 :tag2}
                </div>
            )
        }
    }

export default Header; //暴露Header组件
```
3. 主件render()
```
    ReactDOM.render(<Content/>, document.getElementById('root'));
```
