# 从文章列表挑战到新闻详情页面（组件自定义属性及获取属性）



模板中加入事件监听函数 是不可以响应的
template只是一个占位符而已，需要在template外面定义一个view
 


自定义属性绑定
data-postId="item.postId"

onTap事件中的
event.currentTarget.dataset.postId


先静后动，先样式后数据

## 配置全局单导航栏颜色



## 对于列表页面添加对应的点击事件

直接在template标签中添加 bindTap事件是无法触发事假的，因为tempalte仅仅起着一个占位符的作用。在显示数据的时候会被对应的模板中的内容替换掉。
所以应该在 template标签外面再加一个view标签，对这个view标签添加tap事件。


## 组件自定义属性

 在 标签中加入 data-postId=""

在onLoad中如何获取

onLoad中提供了一个event可以获取传送的参数。
 event.currentPrepare.dataset.postid

> 自定义属性建议使用 data-xx-xx “烤串”的方式命名，在js中会自动转成驼峰的命名方式。
> 如果自定义属性命名的是data-postId， 那么在js中对应的为 postid, 连字符关联的名称都会转为小写的。



页面传参 
url?id=""


Page中接受

onLoad option
var postId = option.id;

  调试工具的Appdata会展示所有打开页面的数据绑定的数据，方便调试

如果在onload方法中，不是异步的去执行一个数据绑定，则不需要使用this.setData方法，只需要对this.data赋值即可实现数据绑定。

异步调用无法保证onload函数在其调用成功之后才去执行


在一般的新闻列表页面中，一般页面都是后台已经静态化好的html然后，
在app使用webview加载， 这个好处是 静态化好的html 性能高，内容比较丰富。


## 缓存storage 基本用法

文章收藏，暂时时候缓存来实现

设置缓存  wx.setStorage() //异步

wx.setStorageSync('String','object/string')  同步


特性：
1、如果用户不主动清楚缓存，缓存会一直存在，
缓存最多不能超过10M。

修改缓存
wx.setStorageSync(); //保持key值相同即可


页面中加载事件
 catchTap oncollectionTap

即使不用框架函数的参数，也都加上

 wx.getStorage('key')  
wx.getStorageSync

删除缓存

wx.removeStorage()

wx.removeStorageSync('key') //

wx.clearStorageSync () //清楚所有缓存

总结
4类操作，8中方法。提供同步异步方法

小程序提倡的是数据优先。
//使用用户是否实现
wx:if="{{collected}}"
wx:else 

 false，或者元素存在则都会为没有收藏状态

 使用小程序实现收藏功能

交互反馈

wx.showToast()
   1000毫秒比较符合用户的习惯
title,duration , icon一般不用
不需用户确认
hideToast( )// 隐藏Toast


wx.showModel 
模态，如果不主动关闭，则不会关闭
需要用户确认

success会接受一个res参数 res.confirm=true为收藏按钮

 var that = this 

 wx.showActionSheet(){ 最大长度为6个

 
res cacel tapIndex 数组元素序号从0 开始