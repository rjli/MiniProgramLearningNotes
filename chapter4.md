# 小程序的Template使用

---
>  通过本片文章，我们将对使用tempalte模的有一个初步了解。
## 分离业务数据
在开始介绍template模板之前，首先我们把数据与业务进行分离。在项目中建立datas文件夹，然后新建一个post_data.js文件，用来模拟从服务器中获取的数据。
<pre>
var local_database = [
  {
    date: "2018-03-29",
    title: "百变狸猫",
    imgSrc: "/images/post/bblm.png",
    avatar: "/images/avatar/1.png",
    content: "那个比宫崎骏还会讲故事的人走了，这部片最适合纪念他",
    reading: "112",
    collection: "96"
  },
  {
    date: "2018-04-12",
    title: "走出非洲",
    imgSrc: "/images/post/zcfz.png",
    avatar: "/images/avatar/2.png",
    content: "一个女人到底要多美才算是真的美丽呢？是需要拥有不老的美丽容颜，还是拥有一段人人羡慕的魔鬼身材？",
    reading: "200",
    collection: "86"
  }
]
</pre>

对于定义好的脚本文件一定要指定一个出口,从而可以使得数据能都输出到别的脚本文件中去,使用moudle.export实现：

	moudule.exports ={
	 postList:local_database
	}


##  使用require.js方法加载js模块文件

在post.js文件中，我们需要使用requir.js来加载刚刚定义好的数据文件。

1、定义变量
<pre>
var  postData = require('../../datas/post_data.js');
</pre> 
> 这需要使用相对路径，如果使用绝对路径会报错Error: module "pages/posts/datas/post_data.js" is not defined

2、在onLoad方法中把请求回来的数据赋值给Page中的data对象。
<pre>
onLoad: function (options) {
    console.log(postsData);
    this.setData({
      postList: postsData.postList
    })
  },
</pre>
swiper-item
仅可放置在<swiper/>组件中，宽高自动设置为100%。

## template模板

小程序的template模板只需要wxml与wxss两个文件，不需要js，以及对应的json文件的。使用模板来完成页面搭建，可以实现一次定义，到处使用，减少了代码的重复性，也提高了开发的效率。


### 创建模板

在post下面新建一个post-item的文件夹，然后建立对应的post-item-template.wxml，post-item-template.wxss文件。  

创建post-item-template.wxml。<template/>内定义代码片段，使用name属性，作为模板的名字。

	<!--电影资讯模板页面  -->
	<template name="posttemplate">
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
	</template>


>1、模板相关的文件命名可以使用-template后缀与非模板的页面进行区别。  
>2、可以看到一个.wxml文件中可以定义多个模板，只需要通过name来区分；

### 使用模板
1、在需要使用的模板页面中引入模板，在页面中加入 

	import src='post-item/post-item-template.wxml'/> 
 	//此处相对路径也可以，绝对路径也可以，建议使用相对路径
2、在需要模板的地方，通过模板名称来引用模板。

	<!--电影资讯列表  -->
	<block wx:for="{{postList}}" wx-item="item">
    	<template is="posttemplate" data="{...{item}}" />
	</block>
>- 使用 is 属性，声明需要的使用的模板
>- 将模板所需要的 data 传入，实现数据绑定，一般我们都会使用列表渲染。
>- data="{{...item}}" 写法是ES6的写法，item是wx:for当前项，... 是展开运算符相当于把当前元素展开，所以在模板中不需要再{{item.date}} 而是直接{{date}} 。

### 样式引入

1、在新建模板的时候同时新建一个post-item-template.wxss的文件，与CSS同样的写法控制样式。   
2、在需要使用模板的页面 .wxss文件中import进来(@import  "相对路径")。  
3、如果模板是对整个小程序的，那么可以直接在app.wxss中引入，这样只需要一次引入，其他文件就不用引入了。

