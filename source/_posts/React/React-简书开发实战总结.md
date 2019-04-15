---
title: React 简书开发实战总结
comments: true
date: 2019-04-15 11:24:54
categories: React
tags: ['React']
---

> Created by sea, 2019.4.7


## 起步，准备工作
### 新建项目、安装脚手架
npx create-react-app jianshu
cd jianshu
npm start

###  绑定git远程仓库
git init
git remote 
git remote add origin git@github.com:Supremeyh/jianshu.git


### 主要技术点
#### index.js
```JavaScript
// src/index.js
import React from 'react';
import ReactDOM from 'react-dom';
import TodoList from './TodoList'

const rootElement = document.getElementById('root')
ReactDOM.render(<TodoList />, rootElement)
```

#### state
```JavaScript
// 基本用法
this.setState({
  inputVal: 'sth...'
})

// 异步state,还可接收回调
this.setState((prevState) => {
  const list = [...prevState.list]
  list.splice(index, 1)
  return { list }
})

this.setState((prevState, props) => ({
  list: [...prevState.list, {text: prevState.inputVal, id: parseInt(prevState.list.length)+1}],
  inputVal: ''
}), () => {
  // cb
})
```

#### ref属性
```JavaScript
// 下面的tar就指向了ref所在的DOM元素，将其赋值给this.input，这样this.input就等价于e.target。
// this.input一旦被定义之后，在整个render方法中都可以获取到。这个属性不推荐使用，因为直接操作DOM不符合React的面向数据的思想。
<input ref={(tar) => {this.input = tar}} onChange={this.handleInputChange}></input>

handleInputChange() {
  this.setState(() => ({
    value: this.input.value
  }))
}
```

#### LifeCycle
* Mounting
getDefaultProps => getInitialState => componentWillMount() => render=> componentDidMount()
* Updating
componentWillReceiveProps(nextProps) => shouldComponentUpdate(nextProps, nextState) => componentWillUpdate(nextProps, nextState) => render => componentDidUpdate(prevProps, prevState)
* Unmounting
componentWillUnmount()


其中，shouldComponentUpdate 比较特殊
shouldComponentUpdate(nextProps, nextState) {  //两个参数分别表示：传进来的props和state
  if (nextProps.content !== this.props.content) {
    return true;
  } else {
    return false;
  }
}


## 正式开发流程和主要依赖
### styled-components
npm i --save styled-components

### normalize.js
使用 styled-components 变成全局样式导入
```JavaScript
// normalize.js
import { createGlobalStyle } from 'styled-components'

const GlobalStyle = createGlobalStyle`
  body {
    margin: 0;
  }
`
export { GlobalStyle }

// src/App.js
import { GlobalStyle }

<GlobalStyle />
<Header />
```

### iconfont.js
使用 styled-components 变成 GlobalIconfontStyle 全局样式导入
```JavaScript
<GlobalStyle />
<GlobalIconfontStyle />
```

### react-transition-group
```JavaScript
// src/common/header/index.js
import { CSSTransition} from 'react-transition-group'

<CSSTransition in={this.state.focus} timeout={200} classNames='slide'>
  <NavSearch></NavSearch>
</CSSTransition>
```

### antd 
npm i antd --save

```JavaScript
import 'antd/dist/antd.css'
import { Input } from 'antd';
<Input placeholder='antd input' />  //注意UI组件首字母大写
```

### redux
npm i redux
![redux 数据流动关系](/images/redux-flow.jpg)

store提供的三种方法：
store.dispatch()  组件向store传递action的唯一方法
store.subscribe() 监听store中的数据，一旦数据变化，就执行这个函数
store.getState()  获取store中的最新的数据。 数据被更新了，那么页面自然也就被更新了
```JavaScript
// 创建store
// src/store/index.js
import { createStore } from 'redux'
import reducer from './reducer';  //笔记本

// Redux设计思想：Web 应用是一个状态机，视图与状态是一一对应的；所有的状态，保存在一个对象里面
const store = createStore(  //创建仓库
  reducer,  //将笔记本传递给仓库
  window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
);

export default store

// 创建reducer
// src/store/reducer.js
const defaultState = {
  inputVal: '',
  list: []
}

const reducer = (state=defaultState, action) => {
  switch(action.type) {
    case CHANGE_INPUT_VALUE:
      const newState = {...prevState, ...action.value}
      newState.inputVal = action.value
      return newState
    case INIT_TODO_LIST:
      const newState = {...prevState,...{ list: action.list }}
      return newState
    default:
      return state
  }
}

export default reducer

// src/Todolist
import store from './store/index'

class TodoList extends Component {
  constructor(props) {
    super(props)
    this.state = store.getState()
    this.handleInputChange = this.handleInputChange.bind(this)
    //subscribe用来监听store，一旦store中存储的数据发生变化，就自动执行这个函数
    store.subscribe(this.handleStoreChange)
  }

   handleInputChange(e) {
     const action = {          
      type: CHANGE_INPUT_VALUE,
      value: e.target.value
    }
    
     //将这个数据变更的信息传递给store。 dispatch是组件向store发送action的唯一方法
    store.dispatch(action)
  }

  handleStoreChange() {
    //store.getState() 用来获取store中最新的数据
    this.setState(store.getState())
  }

  // redux发送异步请求获取数据的代码放在componentDidMount函数里
  componentDidMount() {
    const action = {
      type: INIT_TODO_LIST
    }
    store.dispatch(action)
  }
}
```
在实际项目中，需要将action的type名提取出来，并将每个action封装。分别创建 actionTypes.js 和 actionCreators.js

### 在容器组件中拆分出UI组件
Todolist就是容器组件，它的render方法中返回了很多的UI组件，所以有必要把他们拆分一下。
拆分后，UI组件里面的this.state都无法获取到了，这时就需要父组件（即容器组件）将this.state和方法作为参数，传递给UI组件。
UI组件中，需要将原先所有的this.state换成this.props，将原先的this.方法名换成this.props.方法名。

若此时仍需要向方法传递一个参数，就不能直接写成这种形式
onClick={this.props.handleDeleteClick(index)}
替换为
onClick={() => { this.props.handleDeleteClick(index) }}
onClick={() => { this.props.handleDeleteClick.bind(this, index) }}

```JavaScript
// src/Todolist
render() {
  return (
    //将this.state、方法都作为参数传递给UI组件
    <TodolistUI
      inputValue={this.state.inputValue}
      list={this.state.list}
      handleInputChange={this.handleInputChange}
      handleBtnClick={this.handleBtnClick}
      handleDeleteClick={this.handleDeleteClick}
    />
  )
}

// src/TodolistUI
import React, { Component, Fragment } from 'react';
import { Input, Button, List } from 'antd';

// 无状态组件(没有state的组件，只有一个render方法)改写成函数，提高性能
const TodoListUI = (props) => {
  return (
    // 改写后的函数接收一个参数props,可以将this.props.x 换成props.x
  )
}
// 或者不用赋值式函数字面量，写成函数声明
function TodoListUI(props) {
  return (
    <Fragment>
      <Input 
        value={props.inputVal}
        onChange={props.handleInputChange} 
        placeholder="input here" 
        style={{width: "300px", marginRight: "15px"}}
      />
      <Button onClick={props.handleBtnClick} type="primary">Submit</Button>
      <List 
        bordered
        dataSource={props.list}
        renderItem={(item, index) => (<List.Item onClick={() => {props.handleItemDel(index)}}>{item}</List.Item>)}>
      </List>
    </Fragment>
  )
}

export default TodoListUI
```

### 使用 react-redux 中间件, 处理异步请求
npm i redux-thunk --save

如果将redux发送异步请求获取数据的代码都放在componentDidMount钩子函数中，随着业务逻辑的增加，这个钩子函数可能会变的越来越臃肿。
可以利用redux-thunk将redux发送异步请求获取数据的代码放在其他函数中。
不使用redux-thunk时，action只能是一个对象，有了redux-thunk之后，action就可以是一个函数了。
如果不使用redux-thunk，并且让action是一个函数的话，就会报以下错误 Error: Actions must be plain objects. Use custom middleware for async actions。就是说：action必须是一个对象
```JavaScript
// src/store/index.js
import { createStore, applyMiddleware, compose } from 'redux'
import reducer from './reducer'
import thunk from 'redux-thunk'

const enhancer = compose(
  applyMiddleware(thunk), 
  window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
)

const store = createStore(reducer, enhancer)

export default store

// src/store/actionCreators.js
import axios from 'axios';

export const getListItem = () => {
  return (dispatch) => {  //store的dispatch方法可以作为一个参数被接收
    axios.get('http://api/get_list')
      .then((res) => {
        const data = res.data
        const action = getInitialListItemAction(data)
        dispatch(action)
      })
  }
}


// src/Todolist.js
import getListItem  from './store/actionCreators';

componentDidMount() {
  const action = getListItem()
  store.dispatch(action)  //当dispatch接收到一个函数参数时，会自动执行这个函数
}

// 代码原理 就是把异步请求数据的代码放在其它函数中，然后在componentDidMount中用dispatch方法执行这个函数即可
```

### 使用 react-saga 中间件, 处理异步请求
npm i redux-saga --save

```JavaScript
// src/store/index.js
import { createStore, applyMiddleware, compose } from 'redux'
import reducer from './reducer'
import createSagaMiddleware from 'redux-saga'
import sagas from './sagas'

const sagaMiddleware = createSagaMiddleware()

const enhancer = compose(
  applyMiddleware(sagaMiddleware),
  window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
)


const store = createStore(reducer, enhancer)

sagaMiddleware.run(sagas)

export default store


// src/store/sagas.js
import axios from 'axios'
import { call, put, takeEvery, takeLatest } from 'redux-saga/effects'
import { GET_INIT_TODO_LIST } from './actionTypes'
import { initTodoListAction } from './actionCreators'

function* getInitTodoList() {
  try {
    const res = yield  axios.get('https://easy-mock.com/mock/5ca47d2fac5abe5a8d89b977/react/api/todolist')
    const list = res.data.list
    const action = initTodoListAction(list)
    yield put(action)
  } catch(e) {
    console.log('error')
  }
}

function* mySaga(params) {
  yield takeEvery(GET_INIT_TODO_LIST, getInitTodoList)
}

export default mySaga

```

### react-redux
npm i react-redux --save

react-redux就是用来将react和redux连接起来。
通过react-redux中的connect方法，将组件Todolist中的state，dispatch映射到props。mapStateToProps和mapDispatchToProps分别实现映射。之后，Todolist组件中所有的 this.state 和 this.方法名，都直接替换成：this.props 和 this.props.方法名。
```JavaScript
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom'
import TodoList from './TodoList'
import { Provider } from 'react-redux'
import store from './store'

const App = (
  <Provider store={store}>
    <TodoList />
  </Provider>
)
const rootElement = document.getElementById('root')

ReactDOM.render(App, rootElement)


// src/TodoList.js
import React, { Component } from 'react'
import { connect } from 'react-redux'

class TodoList extends Component {
  render() {
    return (
      // ...
    )
  }
}

const mapStateToProps = (state) => {
  return {
    inputVal: state.inputVal,
    list: state.list
  }
}

const mapDispatchTpProps = (dispatch) => {
  return {
    changeInputVal(e) {
      const action = changeInputValAction(e.target.value)
      dispatch(action)
    }
  }
}

// TodoList是个UI组件，最终导出的是 用connect集成 TodoList UI组件、数据和业务逻辑 而 包装成的容器组件
export default connect(mapStateToProps, mapDispatchTpProps)(TodoList)
```


### immutable、redux-immutable
npm i immutable
npm i redux-immutable

用于将对象设置为不可更改。可用于保护state，state是不允许修改的，为了防止误修改，可以借助immutable库。
```JavaScript
// src/pages/home/store/reducer.js
import { fromJS } from 'immutable'

const defaultState = fromJS({
  topicList: [],
  articleList: [],
  recommendList: [],
  articlePage: 1,
  showScroll: false
})


const changeHomeData = (state, action) => {
  return state.merge({
    topicList: fromJS(action.topicList),
    articleList: fromJS(action.articleList),
    recommendList: fromJS(action.recommendList)
  })
}

const getMoreArticleList = (state, action) => {
  const newArticleList = [...state.get('articleList'), ...fromJS(action.list)]
  const nextPage = action.nextPage
  return state.merge({
    articleList: newArticleList,
    articlePage: nextPage
  })
}

const reducer = (state=defaultState, action) => {
  switch(action.type) {
    case CHANGE_HOME_DATA:
      return changeHomeData(state, action)
    case GET_MORE_ARTICLE_LIST:
      return getMoreArticleList(state, action)
    case CHANGE_SCROLLTOP_SHOW:
      return state.set('showScroll', action.flag)
    default:  
      return state
  }
}

export default reducer
```

使用，redux-immutable
```JavaScript
// src/store/reducer.js
import { combineReducers } from 'redux'
// 替换为
import { combineReducers } from 'redux-immutable'


// 使用redux-immutable之后, 就不能使用类似
this.state.header.inputVal
// 替换为
this.state.get('header').get('inputVal')
this.state.getIn(['header', 'inputVal'])  // 简写
```

### redux 模块划分，拆分reducer
随着业务逻辑的增加，代码会变的愈来愈多，reducer会变得越来越大，所以最好将其拆分成不同的块，每个块对应一个reducer.js文件。
```JavaScript
// src/store/reducer.js
import { combineReducers } from 'redux-immutable'
import { reducer as headerReducer } from '../common/header/store'
import { reducer as loginReducer } from '../pages/login/store'

const reducer = combineReducers({
  header: headerReducer,
  login: loginReducer
})

export default reducer


// src/pages/login/index.js
const mapStateToProps = (state) => ({
  isLogin: state.getIn(['login', 'login'])
})

```

### react-router
npm install react-router-dom

作用就是根据不同的url，显示不同的页面信息
```JavaScript
import { BrowserRouter as Router, Route, Link } from 'react-router-dom'

<Router>
  <Route path='/' exact cmponent={Home}></Route>
  <Route path='/about/' exact cmponent={About}></Route>
</Router>

<Link to='/'>Jump to</Link>
```

### PureComponent
使用 PureComponent 代替 Component，需要immutable， 可提升性能。


### 异步组件和withRouter
npm i react-loadable 

```JavaScript
// /pages/detail/loadable.js
import Loadable from 'react-loadable'

const LoadableComponent = Loadable({
  loader: () => import('./index'),
  loading: Loading  // loading 效果
})

const Loading = () => {
  return <div>loading...</div>
}

export default () => <LoadableComponent />


// /pages/detail/index.js
import { withRouter } from 'react-router-dom'

export default connent(null, null)(withRouter(Detail))


// App.js
// import Home from './pages/detail', 替换为
import Detail from './pages/detail/loadable'
```

