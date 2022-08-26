<a name="295c764d"></a>
# 如何创建一个React-App
 [Create React App](https://github.com/facebook/create-react-app)
<a name="4648a1c1"></a>
# 知识点
<a name="JSX"></a>
## React基础
<a name="iGPfs"></a>
### 当以遍历形式在JSX中实现列表渲染时，需要为遍历项添加 `key` 属性: 

   1. `key` 在 `HTML` 结构中是看不到的，是 `React` 内部用来进行性能优化时使用
   1. `key` 在当前列表中要唯一的字符串或者数值（`String/Number`）
   1. 如果列表中有像 `id` 这种的唯一值，就用 `id` 来作为 `key` 值
   1. 如果列表中没有像 `id` 这种的唯一值，就可以使用 `index`（下标）来作为 `key` 值
```jsx

function App() {
  return (
    <div className="App">
      <ul>{songs.map(item => <li key = {item.id} > {item.name} </li>)}</ul>
    </div>
  )
}
```
<a name="HQcnD"></a>
###  根据是否满足条件生成HTML结构，比如Loading效果。可以使用`三元运算符`或`逻辑与(&&)运算符`: 
```jsx
<div className="App">
  {flag ? 'react' : 'vue'}
  {flag ? <span>this is span</span> : null}
</div>
```
 
<a name="b2L5a"></a>
###  在实际应用时的注意事项 

   1. JSX必须有一个根节点，如果没有根节点，可以使用`<></>`（幽灵节点）替代
   1. 所有标签必须形成闭合，成对`闭合`或者`自闭合`都可以
   1. JSX中的语法更加贴近JS语法，属性名采用驼峰命名法  `class -> className`  `for -> htmlFor`
   1. JSX支持多行（换行），如果需要换行，需使用`()` 包裹，防止bug出现

---

<a name="ycjvs"></a>
## 阶段小练习
<a name="pE4eR"></a>
#### 练习目标

1. 拉取准备好的项目模块到本地 ，安装依赖，run起来项目<br />[https://gitee.com/react-course-series/react-jsx-demo](https://gitee.com/react-course-series/react-jsx-demo) 
1.  按照图示，完成 `评论数据渲染`  `tab内容渲染`  `评论列表点赞和点踩`  三个视图渲染 

![](https://cdn.nlark.com/yuque/0/2022/png/274425/1654489923710-91b3abce-8f29-4550-9a3d-ff11f70efa55.png#crop=0&crop=0&crop=1&crop=1&from=url&id=Y4Bom&margin=%5Bobject%20Object%5D&originHeight=342&originWidth=787&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)
<a name="ZeqzW"></a>
#### 参考答案
```jsx
import './index.css'
import React from "react";
import avatar from './images/avatar.png'
// 依赖的数据
const state = {
    // hot: 热度排序  time: 时间排序
    tabs: [
        {
            id: 1,
            name: '热度',
            type: 'hot'
        },
        {
            id: 2,
            name: '时间',
            type: 'time'
        }
    ],
    active: 'time',
    list: [
        {
            id: 1,
            author: '刘德华',
            comment: '给我一杯忘情水',
            time: new Date(),
            // 1: 点赞 0：无态度 -1:踩
            attitude: 1
        },
        {
            id: 2,
            author: '周杰伦',
            comment: '哎哟，不错哦',
            time: new Date(),
            // 1: 点赞 0：无态度 -1:踩
            attitude: 0
        },
        {
            id: 3,
            author: '五月天',
            comment: '不打扰，是我的温柔',
            time: new Date(),
            // 1: 点赞 0：无态度 -1:踩
            attitude: -1
        }
    ]
}
const num_comments = state.list.length
function formatTime(time){
    return `${time.getFullYear()}-${time.getMonth()+1}-${time.getDate()} ${" "} ${time.getHours()}:${time.getMinutes()}`
}
function App () {
    return (
        <div className="App">
            <div className="comment-container">
                {/* 评论数 */}
                <div className="comment-head">
                    <span>{num_comments} 评论</span>
                </div>
                {/* 排序 */}
                <div className="tabs-order">
                    <ul className="sort-container">
                        {state.tabs.map(item=><li key={item.id} className={item.type === state.active?'on':''}>按{item.name}排序</li>
                        )}
                    </ul>
                </div>

                {/* 添加评论 */}
                <div className="comment-send">
                    <div className="user-face">
                        <img className="user-head" src={avatar} alt="" />
                    </div>
                    <div className="textarea-container">
            <textarea
                cols="80"
                rows="5"
                placeholder="发条友善的评论"
                className="ipt-txt"
            />
                        <button className="comment-submit">发表评论</button>
                    </div>
                    <div className="comment-emoji">
                        <i className="face"></i>
                        <span className="text">表情</span>
                    </div>
                </div>

                {/* 评论列表 */}
                <div className="comment-list">
                    {state.list.map( item => (
                        <div  className="list-item">
                            <div className="user-face">
                                <img className="user-head" src={avatar} alt="" />
                            </div>
                            <div className="comment">
                                <div className="user">{item.author}</div>
                                <p className="text">{item.comment}</p>
                                <div className="info">
                                        <span className="time" id='date'>
                                            {formatTime(item.time)}
                                        </span>
                                    <span className={item.attitude===1?'like liked':'like'}>
                                                <i className="icon" />
                                        </span>
                                    <span className={item.attitude===-1?'hate hated':'hate'}>
                                                <i className="icon" />
                                         </span>
                                    <span className="reply btn-hover">
                                            删除
                                        </span>
                                </div>
                            </div>
                        </div>
                    ))}

                </div>
            </div>
        </div>
    )
}
export default App
```
```jsx
import React from 'react'
import ReactDOM from 'react-dom'
import App from './App'

const root = ReactDOM.createRoot(document.getElementById('root'))
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
)
```
<a name="33a0dcff"></a>
## React组件基础（函数、类）

<a name="iTF5B"></a>
### 函数组件约定说明 

   1. 组件的名称 **必须首字母大写** ，react内部会根据这个来判断是组件还是普通的HTML标签
   1. 函数组件 **必须有返回值** ，表示该组件的 UI 结构；如果不需要渲染任何内容，则返回 null
   1. 组件就像 HTML 标签一样可以被渲染到页面中。组件表示的是一段结构内容，对于函数组件来说，渲染的内容是函数的 **返回值** 就是对应的内容
   1. 使用函数名称作为组件标签名称，可以成对出现也可以自闭合
```jsx
function HelloFn () { 
  return <div>这是我的第一个函数组件!</div> 
}
// 定义类组件
function App () {
  return (
    <div className="App">
      {/* 渲染函数组件 */}
      <HelloFn />
      <HelloFn></HelloFn>
    </div>
  )
}
export default App
```
<a name="wbWcz"></a>
###  类组件约定说明 

   1. 类名称也**必须以大写字母开头**
   1. 类组件应该继承 `React.Component` 父类，从而使用父类中提供的方法或属性
   1. 类组件必须提供 `render`方法且 **必须有返回值**，表示该组件的 UI 结构
```jsx
// 引入React
import React from 'react'

// 定义类组件
class HelloC extends React.Component {
  render () {
    return <div>这是我的第一个类组件!</div>
  }
}

function App () {
  return (
    <div className="App">
      {/* 渲染类组件 */}
      <HelloC />
      <HelloC></HelloC>
    </div>
  )
}
export default App
```
<a name="hkmzF"></a>
###  组件事件绑定 

   1.  如何绑定事件 
      - 语法：on + 事件名称 = { 事件处理程序 } ，比如：`<div onClick={ onClick }></div>`
      - 采用驼峰命名法，比如`onMouseEnter、onFocus`
```jsx

function HelloFn () {
  // 定义事件回调函数
  const clickHandler = () => {
    console.log('事件被触发了')
  }
  return (
    //绑定事件
    <button onClick={clickHandler}>click me!</button>
  )
}

////// 类组件

// 整体的套路都是一致的 和函数组件没有太多不同
// 唯一需要注意的 因为处于class 类环境下 所以定义事件回调函数 以及 绑定它写法上有不同
// 定义的时候: class Fields语法  
// 使用的时候: 需要借助this关键词获取
// 注意事项: 之所以要采取class Fields写法是为了保证this的指向正确 永远指向当前的组件实例

import React from "react"
class CComponent extends React.Component {
  // class Fields
  clickHandler = (e, num) => {
    // 这里的this指向的是正确的当前的组件实例对象
    // 可以非常方便的通过this关键词拿到组件实例身上的其他属性或者方法
    console.log(this)
  }
  
  clickHandler1 () {
    // 这里的this 不指向当前的组件实例对象  undefined 存在this丢失问题
    console.log(this)
  }
  
  render () {
    return (<div>
        <button onClick={(e) => this.clickHandler(e, '123')}>click me</button>
        <button onClick={this.clickHandler1}>click me</button>
      </div>
           )}}

function App () {
  return (<div><CComponent /></div>)
}
  export default App
```

   2. 获取事件对象
```jsx
function HelloFn() {
  //事件回调
  const clickHandler = (e,msg) => {
    //关闭对象默认跳转, 点击百度不会跳转
    e.preventDefault()
    console.log('事件被触发了', msg)
  }
  return (<a href="https://www.baidu.com/" onClick={(e)=>clickHandler(e,'You can not leave.')}>百度</a>)
  }
```
<a name="assVL"></a>
###  组件状态 (初始化状态->读取状态->修改状态->影响视图) 

   1. 定义状态必须通过`state`实例属性的方法 提供一个对象 名称是固定的`state`
   1. 修改state中任何值，都必须走`setState`方法
   1. 类组件很少使用了，函数组件偏多
   1. 回调函数必须以 `funcName = () =>{}` 形式给出
```jsx
import React from "react";

class TestComponent extends React.Component {
  state={
    //初始状态
    name:'the Communist Party. ',
    oath:' ',
    count: 0
    
  }
  partyMemberOath=()=>{
    //修改状态
    //注意：不可直接修改状态值,需要调用setState方法
    this.setState({
      oath : 'Serve the people wholeheartedly!',
      count: this.state.count+1
    }
                 )
  }
  render() {
    return (
      <div>
        //使用状态
        <li>Party:{this.state.name}</li>
        <li>The oath:{this.state.oath}</li>
        //影响视图
        <button onClick = {this.partyMemberOath}>Say {this.state.count}</button>
      </div>
      
    )
  }
}
```
<a name="SE9vX"></a>
###  状态不可变说明
 `目标任务:`  能够理解不可变的意义并且知道在实际开发中如何修改状态<br />**概念**：不要直接修改状态的值，而是基于当前状态创建新的状态值
```jsx
...
state = {
  count: 0,
  list:[1,2,3],
  person:{
    name:'jack',
    age:20
  }
}
//列表添加、修改
this.setState({
    count: this.state.count + 1
  
    // 遍历原列表，在其基础上添加
    list: [...this.state.list, 4, 5],
    
    // 覆盖原来的属性 就可以达到修改对象中属性的目的
    person: {
       ...this.state.person,
       
       name: 'rose'
    }
})
//列表删除 (删掉2)
this.setState({
    list: this.state.list.filter(item=> item !== 2),
    }
})
...
```
<a name="yqxxE"></a>
###  受控组件 

   1. 在组件的state中声明一个组件的`状态数据`
   1. 将`状态数据`设置为`input`标签元素的`value`的值
   1. 为`input`添加`change`事件，在事件处理程序中，通过事件对象`e`获取到当前文本框的值
   1. 调用`setState`方法，将文本框内的值设为`state`状态的最新值
```jsx
class TestComponent extends React.Component {
    state={
        //初始状态
        name:'the Communist Party. ',
        oath:' ',
        count:0,
        content:'Initial Content'

    }
    partyMemberOath=()=>{
        //修改状态
        //注意：不可直接修改状态值,需要调用setState方法
        this.setState({
            oath : 'Serve the people wholeheartedly!',
            count: this.state.count+1
            }
        )
    }
    showContent=(e)=>{
        this.setState({
            content : e.target.value
        })
    }
    render() {
        //使用状态
        return (
            //幽灵标签，防止两个并行div标签导致的报错
            <>
                <div>
                    <li>Party:{this.state.name}</li>
                    <li>The oath:{this.state.oath}</li>
                    <button onClick = {this.partyMemberOath}>{this.state.count} Say</button>
                </div>
                <div>
                    Content:{this.state.content}
                </div>
                <input type={"password"} value={this.state.content} onChange = {this.showContent}/>
            </>
        )
    }
}
```
<a name="H8hAb"></a>
## 阶段小练习
<a name="gpUbC"></a>
### 练习目标

1. 拉取项目模板到本地，安装依赖，run起来项目<br />[https://gitee.com/react-course-series/react-component-demo](https://gitee.com/react-course-series/react-component-demo) 
1.  完成tab点击切换激活状态交互 
1.  完成发表评论功能<br />注意：生成独立无二的id 可以使用  uuid 包  `yarn add uuid`

![image.png](https://cdn.nlark.com/yuque/0/2022/png/32511504/1661395579459-ee6ec496-2ad1-4daf-abb7-5ec0f0c2d07b.png#clientId=uaef41dc2-1207-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=714&id=u9c8ceca5&margin=%5Bobject%20Object%5D&name=image.png&originHeight=892&originWidth=1544&originalType=binary&ratio=1&rotation=0&showTitle=false&size=112856&status=done&style=none&taskId=u05e15c54-c057-4b6c-a76c-11e89968fbe&title=&width=1235.2)
<a name="GWHoS"></a>
#### 练习-Tab切换列表

1. 为`<li><li/>`添加 `onClick`属性
1.  编写组件  `tabChange` 
1.  完成事件绑定 `onClick={()=>this.tabChange(item.type)`
```jsx
...
tabChange = (type) => {
        this.setState({
            active : type
        })
    }
...
...
{/* 排序 */}
<div className="tabs-order">
  <ul className="sort-container">
  {this.state.tabs.map(item=> <li 
                              onClick={()=>this.tabChange(item.type)} 
                              key={item.id} 
                              className={item.type === this.state.active?'on':''}>
                              按{item.name}排序
                             </li>
  )}
  </ul>
</div>
...
```
<a name="dgJIk"></a>
#### 练习-发表评论

1. 为`state`添加`输入框<textarea/>`内容变量 `comment`
1. 绑定`onChange`事件(可添加`onClick`增加用户友好)实时更改`state.comment`
1. 为`发表评论`添加`<button/>`并绑定`onClick`事件更新`state.list`
```jsx
...
 state = {
        // hot: 热度排序  time: 时间排序
        tabs: [
            {
                id: 1,
                name: '热度',
                type: 'hot'
            },
            {
                id: 2,
                name: '时间',
                type: 'time'
            }
        ],
        active: 'time',
        list: [
            {
                id: 1,
                author: '刘德华',
                comment: '给我一杯忘情水',
                time: new Date(),
                // 1: 点赞 0：无态度 -1:踩
                attitude: 1
            },
            {
                id: 2,
                author: '周杰伦',
                comment: '哎哟，不错哦',
                time: new Date(),
                // 1: 点赞 0：无态度 -1:踩
                attitude: 0
            },
            {
                id: 3,
                author: '五月天',
                comment: '不打扰，是我的温柔',
                time: new Date(),
                // 1: 点赞 0：无态度 -1:踩
                attitude: -1
            }
        ],
        comment:'请输入内容',
        ifClick: false
    }
...
//提交组件
submitComment = () => {
        this.setState({
            list:[...this.state.list,{
                id:this.state.list.length+1,
                author:"周杰伦",
                comment:this.state.comment,
                time:new Date(),
                attitude:0
            }]

        })
    }
//输入框内容更改
contentChangeBegin = () => {
    if(!this.state.ifClick){
         this.setState({
                comment: '',
                ifClick: true
         })
    }
}
contentChangIng = (e) => {
        this.setState({
            comment:e.target.value
        })
    }
//添加评论
<>
<div className="comment-send">
     <div className="user-face">
          <img className="user-head" src={avatar} alt="" />
     </div>
     <div className="textarea-container">
     {/*输入框*/}
     <textarea
      cols="80"
      rows="5"
      className="ipt-txt"
      value={this.state.comment}
      onClick={this.contentChangeBegin}
      onChange={this.contentChangIng}
      />
     <button
      className = "comment-submit"
      onClick={this.submitComment}
     >
     发表评论
     </button>
     </div>
     <div className="comment-emoji">
       <i className="face"></i>
       <span className="text">表情</span>
       </div>
</div>
</>
```
<a name="KxbEz"></a>
#### 发表评论-为每条评论唯一添加ID标识符

1. `uuid：npm/yarn add uuid`包可独一无二生成 `id`
1. 引入 `import {v4 as uuid} from 'uuid'` 使用 `uuid()`
```jsx
import {v4 as uuid} from 'uuid'
...
list: [
            {
                id: uuid(),
                author: '刘德华',
                comment: '给我一杯忘情水',
                time: new Date(),
                // 1: 点赞 0：无态度 -1:踩
                attitude: 1
            },
            {
                id: uuid(),
                author: '周杰伦',
                comment: '哎哟，不错哦',
                time: new Date(),
                // 1: 点赞 0：无态度 -1:踩
                attitude: 0
            },
            {
                id: uuid(),
                author: '五月天',
                comment: '不打扰，是我的温柔',
                time: new Date(),
                // 1: 点赞 0：无态度 -1:踩
                attitude: -1
            }
        ],
...
submitComment = () => {
        this.setState({
            list:[...this.state.list,{
                id:uuid(),
                author:"周杰伦",
                comment:this.state.comment,
                time:new Date(),
                attitude:0
            }]

        })
    }
...
```
<a name="Fzprs"></a>
#### 练习-删除评论
```jsx
...
contentDelete = (id) => {
        this.setState({
            list:this.state.list.filter(item=>item.id !== id)
        })
    }
...
<span
  onClick={()=>this.contentDelete(item.id)}
  className="reply btn-hover">
  删除
</span>
...

```
<a name="J7ZL9"></a>
#### 练习-切换点赞/踩
```jsx
...
//改变list某个值 需要 map 遍历再筛选id再修改
toggleLike = (curItem) => {
        const {attitude,id} = curItem
        this.setState({
            list:this.state.list.map(item => {
                if(item.id===id){
                    return {
                        ...item,
                        attitude: attitude=== 1?0:1
                    }
                }
                else{
                    return item
                }
            })
        })
    }
toggleHate = (curItem) => {
        const {attitude,id} = curItem
        this.setState({
            list:this.state.list.map(item => {
                if(item.id===id){
                    return {
                        ...item,
                        attitude: attitude=== -1?0:-1
                    }
                }
                else{
                    return item
                }
            })
        })
    }
...
<span onClick = {() => this.toggleLike(item)} 
   className={item.attitude=== 1?'like liked':'like'}>
  <i className="icon" />
</span>
<span onClick = {() => this.toggleHate(item)} 
   className={item.attitude=== -1?'like liked':'like'}>
  <i className="icon" />
</span>

```
<a name="Nvf0f"></a>
## React组件通信（OOP 函数、类之相间调用）
<a name="H5H5G"></a>
### 为什么需要组件通信、步骤是啥

1. 每个组件是个`独立封闭`的个体，只能使用自己的数据`state`
1. 组件化开发（OOP开发）过程中，完整的功能会被拆分，不可避免的需要数据交互
1. 组件间关系包括：
   1. `父子关系`-**最重要的**
   1. `兄弟关系`-自定义事件模式产生技术方法`eventBus` /通过同一个父组件通信
   1. `其它关系`-**mobx 、redux、zustand**
<a name="GEtCK"></a>
### 父传子
![](https://cdn.nlark.com/yuque/0/2022/png/274425/1654490432739-ea283505-3ddd-4403-9fba-7735b04b451e.png#crop=0&crop=0&crop=1&crop=1&from=url&id=dTuhG&margin=%5Bobject%20Object%5D&originHeight=785&originWidth=1391&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

1. 父组件提供要传递的数据 - `state`
1. 给子组件标签添加属性值为 state 中的数据 
1. 子组件中通过`props`接收父组件中传过来的数据
   1. 类组件使用 this.props 获取 props 对象
   1. 函数组件通过参数获取 props 对象
```jsx
import React from 'react'

// 函数式子组件
function FSon(props) {
  console.log(props)
  return (
    <div>
      子组件1
      {props.msg}
    </div>
  )
}

// 类子组件
class CSon extends React.Component {
  render() {
    return (
      <div>
        子组件2
        {this.props.msg}
      </div>
    )
  }
}
// 父组件
class App extends React.Component {
  state = {
    message: 'this is message'
  }
  render() {
    return (
      <div>
        <div>父组件</div>
        <FSon msg={this.state.message} />
        <CSon msg={this.state.message} />
      </div>
    )
  }
}

export default App
```
<a name="ytMZC"></a>
### props说明

1. `props`是只读对象（readOnly）,子组件无法更改`props`
1. `props`可以是传递任意数据：对象`{this.state.message}`、函数`{()=>{console.log(*)}}`、JSX `{<span>***</span>}`
<a name="QgDl4"></a>
### 子传父
![](https://cdn.nlark.com/yuque/0/2022/png/274425/1654490502446-0596a169-847f-4446-91ce-a9a0237a9074.png#crop=0&crop=0&crop=1&crop=1&from=url&id=Ysu9Q&margin=%5Bobject%20Object%5D&originHeight=775&originWidth=1406&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)
> **口诀：** 父组件给子组件传递回调函数，子组件调用

<a name="ZqQig"></a>
#### 实现步骤

1. 父组件提供一个 `回调函数`-用于`接收数据`
1. 将`回调函数`作为属性的值传递给子组件
1. 子组件通过`props`调用`回调函数`
1. 将子组件中的数据作为参数传递给`回调函数`
```jsx
import React from 'react'

// 子组件
function Son(props) {
  function handleClick() {
    // 调用父组件传递过来的回调函数 并注入参数
    props.changeMsg('this is newMessage')
  }
  return (
    <div>
      {props.msg}
      <button onClick={handleClick}>change</button>
    </div>
  )
}


class App extends React.Component {
  state = {
    message: 'this is message'
  }
  // 提供回调函数
  changeMessage = (newMsg) => {
    console.log('子组件传过来的数据:',newMsg)
    this.setState({
      message: newMsg
    })
  }
  render() {
    return (
      <div>
        <div>父组件</div>
        <Son
          msg={this.state.message}
          // 传递给子组件
          changeMsg={this.changeMessage}
        />
      </div>
    )
  }
}

export default App
```
<a name="weS3W"></a>
### 兄弟间通信
![](https://cdn.nlark.com/yuque/0/2022/png/274425/1654490527043-7acbe144-a306-40af-a878-3a7f4ba3a599.png#crop=0&crop=0&crop=1&crop=1&from=url&id=cpLAw&margin=%5Bobject%20Object%5D&originHeight=752&originWidth=1372&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)
> 通过状态提升机制，利用共同的父组件实现兄弟通信

<a name="Lekkx"></a>
#### **实现步骤**

1. 将`共享状态`提升到最近的`公共父组件`中，由`公共父组件`管理这个状态 
```jsx
{/* 接收数据的组件 */}
<SonA msg={this.state.message} />
{/* 修改数据的组件 */}
<SonB changeMsg={this.changeMsg} />
```

   - 提供`共享状态`
   - 提供操作`共享状态`的方法
2. 要接收数据状态的子组件通过 `props` 接收数据
2. 要传递数据状态的子组件通过`props`接收方法，调用方法传递数据
```jsx
import React from 'react'

// 子组件A
function SonA(props) {
  return (
    <div>
      SonA
      {props.msg}
    </div>
  )
}
// 子组件B
function SonB(props) {
  return (
    <div>
      SonB
      <button onClick={() => props.changeMsg('new message')}>changeMsg</button>
    </div>
  )
}

// 父组件
class App extends React.Component {
  // 父组件提供状态数据
  state = {
    message: 'this is message'
  }
  // 父组件提供修改数据的方法
  changeMsg = (newMsg) => {
    this.setState({
      message: newMsg
    })
  }

  render() {
    return (
      <>
        {/* 接收数据的组件 */}
        <SonA msg={this.state.message} />
        {/* 修改数据的组件 */}
        <SonB changeMsg={this.changeMsg} />
      </>
    )
  }
}

export default App
```
<a name="L2y0y"></a>
### 跨组件通信Context
![](https://cdn.nlark.com/yuque/0/2022/png/274425/1654490557423-1b93cabb-8bb8-4d6d-91f5-77c5cbddf105.png#crop=0&crop=0&crop=1&crop=1&from=url&id=v3eVI&margin=%5Bobject%20Object%5D&originHeight=755&originWidth=1307&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)
> 上图是一个react形成的嵌套组件树，如果我们想从App组件向任意一个下层组件传递数据，该怎么办呢？目前我们能采取的方式就是一层一层的props往下传，显然很繁琐
> 那么，Context 提供了一个**无需为每层组件手动添加 props，就能在组件树间进行数据传递的方法**

<a name="JwMex"></a>
#### 实现步骤

1. 创建`Context`对象 导出`Provider`和 `Consumer`对象
```jsx
const {Provider,Consumer} = createContext()
```

2. 使用`Provider`包裹上层组件提供数据
```jsx
<Provider value = {this.state.message}>
	{/*根组件*/}
</Provider>	
```

3. 需要用到数据的 组件使用 Consumer 获取包裹的数据
```jsx
<Consumer >
    {value => /* 基于 context 值进行渲染*/}
</Consumer>
```
<a name="WJ7Kk"></a>
## 阶段小练习
<a name="ux1bD"></a>
## React组件进阶
<a name="zcdIr"></a>
## 阶段总结
<a name="uvPYj"></a>
## Hooks基础
<a name="nHIyR"></a>
## Hooks进阶
