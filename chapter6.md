# 电影列表页面

> 请求豆瓣的api:http://t.yushu.im
>
> 发送http请求中：referer  user-agent 无法自己去更改



利用时间冒泡的机制，在父元素加点击事件



target和currentTarget的区别

target指的是当前点击的组件，currentTarget指的是事件捕获的组件





tabBar

template 模板嵌套

justify-content:space-between (无法使用vertical-middle)

space-arroud 是平均分布 center为居中



## Restful API 加载数据

restful api json

新浪微博，语义化的url

豆瓣api

特点

 1、url 里面再带描述信息  ，语义化 

2、接口的粒度 

3、微服务很大程度是为了接口的粒度



wx.request({}) //类似于ajax

header：CONTENT-TYPE :application/json  CONTENT-TYPE :json 





异步请求数据的时候，一定要在data中初始化，如果数据为一个对象的，赋值为{}，否则页面绑定数据的时候后报错



豆瓣api请求的时候出现403.需要用nginx代理来实现,参考页面  （https://github.com/zce/douban-api-proxy）







