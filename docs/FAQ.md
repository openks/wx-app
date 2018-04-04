## 如何实现类型vue中filter的功能

先定义filter文件
```js
// src/utils/filters.js
let gander = {
    1: '男',
    2: '女'
}
let ganderFilter = value => {
    // console.log(a)
    return gander[value] || ''
}
export {
    ganderFilter,
}
```
实例化之前使用filter
```js
// src/main.js
import * as filter from '@/utils/filters.js'
// 添加filter
Object.keys(filter).forEach(k => {
    Vue.filter(k, filter[k])
})
new Vue({
    el: '#app',
    router,
    store,
    template: '<App/>',
    components: { App }
})
```
在页面上使用filter
```html
 <div> {{ gander | ganderFilter}}</div>
```

而小程序里就没法这么使用  
具体实现方案如下：
定义工具类并作为调用函数在需要的时候调用  
```js
//utils/util.js
let arr = {
  0: "女",
  1: "男"
}

const formateGender = n => {
  return arr[n]||"未知"
}

module.exports = {
  formatTime: formatTime,
  formateGender
}
```
```js
//index.js
//通过把处理后的字段作为新字段保存，在需要的时候展示新字段即可
import { formateGender } from '../../utils/util.js'
Page({
  data: {
    userInfo: {},
  },
  onLoad: function () {
      this.setData({
        userInfo: app.globalData.userInfo,
        'userInfo.fgender': formateGender(app.globalData.userInfo.gender),
        hasUserInfo: true
      })
  },
})
```
```html
<!--index.wxml-->
<view class="container">
  <view class="userinfo">
    <block >
      <text class="userinfo-nickname">{{userInfo.nickName}}</text>
      <text class="userinfo-nickname">{{userInfo.fgender}}</text>
    </block>
  </view>
</view>
```

