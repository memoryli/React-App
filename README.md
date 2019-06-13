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
