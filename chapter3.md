# 编写电影资讯列表页面
---


## js文件的结构和的生命周期

介绍数据绑定之前，我们需要了解一个js文件的结构和的生命周期。
### js文件的文件结构
我们首先了解一下小程序默认的js的文件结构：
<pre>
Page({
  data: {}, //页面的初始数据
  onLoad: function (options) {},// 生命周期函数--监听页面加载
  onReady: function () {}, //生命周期函数--监听页面初次渲染完成
  onShow: function () {},// 生命周期函数--监听页面显示
  onHide: function () {},//生命周期函数--监听页面隐藏
  onUnload: function () {},//生命周期函数--监听页面卸载
  onPullDownRefresh: function () {},//页面相关事件处理函数--监听用户下拉动作
  onReachBottom: function () {},//页面上拉触底事件的处理函数
  onShareAppMessage: function () {}//用户点击右上角分享
})
</pre>

Page() 函数用来注册一个页面。接受一个 object 参数，其指定页面的初始数据、生命周期函数、事件处理函数等。。

### 生命周期函数

- onLoad: 页面加载
> 一个页面只会调用一次，可以在 onLoad 中获取打开当前页面所调用的 query 参数。  
> 页面的初始化操作是在onLoad函数中进行的。 
 
- onShow: 页面显示
> 每次打开页面都会调用一次。

- onReady: 页面初次渲染完成
> 一个页面只会调用一次，代表页面已经准备妥当，可以和视图层进行交互。  
> 对界面的设置如wx.setNavigationBarTitle请在onReady之后设置。详见生命周期

- onHide: 页面隐藏
> 当navigateTo或底部tab切换时调用。

- onUnload: 页面卸载
> 当redirectTo或navigateBack的时候调用。



## 数据绑定基础

如果在html中想要把变量绑定到一个标签上面，我们首先需要获取到这个dom元素，然后在对其赋值。而 
在小程序中提供了一种数据绑定机制，而无需在使用dom，这种方式类似于angularJS。  

不同之处在于小程序中的数据绑定是单向数据绑定，而angularJs中使用的是双向的数据绑定。

在小程序中如果数据在脚本文件向wxml文件传递的时候发生改变，那么wxml中的数据是会随之改变的，但是如果数据是在wsml中发生改变的，脚本文件中的数据并不自动的会发生改变，如果想要改变则需要使用到事件
  
下面我们利用数据绑定实现一个简单的列表页面。

### 数据初始化

- 小程序提供了setData函数用于将数据从逻辑层发送到视图层（异步），同时改变对应的 this.data 的值（同步）。 

![avatar](images/2-setdata.png)

> 注意： 
 
>- 初始化数据将作为页面的第一次渲染。data 将会以 JSON 的形式由逻辑层传至渲染层。   
>- 直接修改 this.data 而不调用 this.setData 是无法改变页面的状态的，还会造成数据不一致。  
>- 单次设置的数据不能超过1024kB，请尽量避免一次设置过多的数据。
>- 请不要把 data 中任何一项的 value 设为 undefined ，否则这一项将不被设置并可能遗留一些潜在问题。 

使用this.setData()对data进行初始化操作：
<pre>
 var postitem={
    date: "2018-03-29",
    title: "百变狸猫",
    imgSrc: "/images/post/bblm.png",
    avatar: "/images/avatar/1.png",
    content: "那个比宫崎骏还会讲故事的人走了，这部片最适合纪念他",
    reading: "112",
    collection: "96"
  }

  this.setData({
      postitem: postitem
  })
</pre>


### 实现数据绑定
通过使用{{变量名}}来实现。

	 <view class='post-container'>
	    <view class="post-author-date">
	      <image src="{{avatar}}" class="post-author"></image>
	      <text class="post-date">{{date}}</text>
	    </view>
	    <text class="post-title">{{title}}</text>
	    <image src="{{imgSrc}}" class="post-image"></image>
	    <text class="post-content">{{content}}</text>
	    <view class="post-like">
	      <image class="post-like-image" src="/images/icon/chat.png"></image>
	      <text class="post-like-font">{{reading}}</text>
	      <image class="post-like-image" src="/images/icon/view.png"></image>
	      <text class="post-like-font">{{collection}}</text>
	    </view>
	 </view>


### 运行结果 
 下面是页面的运行效果：  
![avatar](images/3-demo.png)

> 图片上部是使用小程序提供的swipper组件实现的轮播图。 具体使用可参考官方api文档。
> 需要注意的是如果想要是swiper-item仅可放置在<swiper/>组件中，宽高自动设置为100%。所以如果想使滑动区域的宽度适配到整个屏幕,一定要为<swiper>添加width:100%。


### 数据绑定扩展

<swiper> 的vertical 置为true, 如果改为false的时候 是不会还原成水平滚的，原因是小程序在解析中字符串的时候为把他解析为一个boolean值，而false不为空，所以认为还是垂直滚动， 但是如果使用数据的绑定机制，使用{{false}}那么就会改成水平的了

控制标签元素的显示和隐藏
使用 wx:if="{{false}}"


在双花括号中可以对字符串进行一个简单的运算
{{"hello" + data}}



## wx-for 展示数据
<block> 把要循环的代码包围起来，从他的作用来看可以把他简单理解为一个括号。

在block中的代码会识别为一个整体

wx:for={{}}

对标签的属性去做数据绑定的一定要加引号，而文本的绑定则是不需要加引号的

wx:for-item="item" ，item可以不指定，默认的就是item


setData({ key:value}) 如果直接把数据元素放进去是取不到的

wx:for-index='index'

>- wx:index="index",指定是当前数据的索引值，从0开始，默认值为index。所以可以不指定 





