# vue-router
vue前端路由的实现原理

## 前端路由介绍
单页面应用成为前端应用的主流形式。前端路由也成了必不可少的一部分，通过改变URL，在不刷新页面的情况下实现视图的更新。**前端路由的核心就是更新视图但不重新请求页面**

## 实现方式
1.利用hash（'#'）
2.利用History interface在HTML5中新增的方法

```javascript
import Vue from 'vue'
import Router from 'vue-router'
import routes from './routes'

Vue.use(Router)

export default new Router({
  mode: 'history',
  routes
})
// 根据mode确定history实际的类并实例化
switch (mode) {
  case 'history':
    this.history = new HTML5History(this, options.base)
    break
  case 'hash':
    this.history = new HashHistory(this, options.base, this.fallback)
    break
  case 'abstract':
    this.history = new AbstractHistory(this, options.base)
    break
  default:
    if (process.env.NODE_ENV !== 'production') {
      assert(false, `invalid mode: ${mode}`)
    }
}
```
vue会根据mode的类型创建不同的的HTML5对象。**HTML5History、HashHistory、AbstractHistory**，而在浏览器环境只有**HTML5History、HashHistory**

## HashHistory
有两个方法HashHistory.push()会往页面栈中添加记录，HashHistory.replace()会替换当前的页面栈。
## HTML5History
History interface是浏览器历史记录栈提供的接口。可以通过go()、back()、forward()等方法读取页面栈完成页面的跳转，返回等操作。同时它还可以通过pushState(), replaceState() 方法对页面栈进行修改。

## hash特点

* \#用来定位在当前网页的位置，改变hash值不会触发http请求，不会对服务端我作用。
* hash的变化会触发hashChange事件，所以可以监听hashChange事件从而进行逻辑处理
* hash的每一次改变都会在浏览器的历史记录中添加一条记录

根据hash以上特点可以完成对页面的前进、后退等一系列的路由跳转工作。

