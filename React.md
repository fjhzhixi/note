# React

## 元素渲染

1. 元素`(element)` : 描述输出内容的`HTML`代码

2. 渲染 : `ReactDOM.render(element, DOM_point)` : 将元素中的内容绑定到某个节点中

   一般统一设置一个根节点,所有`React`管理的节点均绑定到其上

## 组件

1. 组件使用`React.createClass()`方法创建 : `var name = React.creatClass()`

2. 组件中的方法 :

   1. `getInitialState()` : 返回组件属性的初始状态
   2. `render()` : **定义渲染函数,在每一次`DOM`值改变时进行渲染**,返回`HTML`代码

3. 使用组件 :

   `<name 属性 : 值 />`

