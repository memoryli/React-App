## react源码解析

## 1、JSX到JavaScript的转换
### 
`
function Comp() {
	return <a>1</a>
}

<div id="div" key="key">
  <span>1</span>
  <span>2</span>
</div>

`
`
"use strict";
//一个组件
function Comp() {
  return React.createElement("a", null, "1");
}

React.createElement("div", {
  id: "div",
  key: "key"
}, React.createElement("span", null, "1"), React.createElement("span", null, "2"));

`
#### Comp要大写，如果改成小写，它会被当成html标签，这样运行会报错，因为没有这样的html标签。

### 2、react-element
### 3、react-component
### 4、context
`
import React from 'react'
import PropTypes from 'prop-types'

const { Provider, Consumer } = React.createContext('default')

class Parent extends React.Component {
  state = {
    childContext: '123',
    newContext: '456',
  }

  getChildContext() {
    return { value: this.state.childContext, a: 'aaaaa' }
  }

  render() {
    return (
      <>
        <div>
          <label>childContext:</label>
          <input
            type="text"
            value={this.state.childContext}
            onChange={e => this.setState({ childContext: e.target.value })}
          />
        </div>
        <div>
          <label>newContext:</label>
          <input
            type="text"
            value={this.state.newContext}
            onChange={e => this.setState({ newContext: e.target.value })}
          />
        </div>
	//新的context方法
        <Provider value={this.state.newContext}>{this.props.children}</Provider>
      </>
    )
  }
}

class Parent2 extends React.Component {
  // { value: this.state.childContext, a: 'bbbbb' }
  getChildContext() {
    return { a: 'bbbbb' }
  }

  render() {
    return this.props.children
  }
}
function Child1(props, context) {
  console.log(context)
  //新的context方法
  return <Consumer>{value => <p>newContext: {value}</p>}</Consumer>
}

Child1.contextTypes = {
  value: PropTypes.string,
}
//老的context方法
class Child2 extends React.Component {
  render() {
    return (
      <p>
        childContext: {this.context.value} {this.context.a}
      </p>
    )
  }
}

// Child2.contextType = Consumer
//老的context方法
Child2.contextTypes = {
  value: PropTypes.string,
  a: PropTypes.string,
}

Parent.childContextTypes = {
  value: PropTypes.string,
  a: PropTypes.string,
}

Parent2.childContextTypes = {
  a: PropTypes.string,
}

export default () => (
  <Parent>
    <Parent2>
      <Child1 />
      <Child2 />
    </Parent2>
  </Parent>
)

`
concurrent-mode
让react的渲染过程能够进行优先级排比，并且能够中断，进行任务调度，将更多的cpu性能分配给优先级较高的

