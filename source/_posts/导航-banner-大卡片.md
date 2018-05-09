---
title: 导航+banner+大卡片
date: 2018-05-09 10:02:05
tags:
---
实现效果如图：
![Alt text](https://i.loli.net/2018/05/09/5af256fd9bf4f.png)
基于上次代码继续添加后的效果。

首先需要把导航条的样式稍作改动，导航条是固定在屏幕的顶部，无论如果滚动屏幕，位置始终不变。为了达到这个效果，我把整个导航栏的DIV做绝对定位position: fixed，但是这样造成导航条的宽度谁整体缩小
如图![Alt text](https://i.loli.net/2018/05/09/5af25992a7480.png)
为了解决宽度的问题，迫不得已引入属性width: 100%，但是这是左右两边的padding位置不对。这是需要在加上一个内部的DIV，在这个内部DIV上加padding才可以达到想要的效果。

添加背景图
因为图片是在一个单独的div内使用背景图的方式引入的，在这个div内没有其他元素，所以需要设定宽度和高度，否则整个div的高度是为零的。css代码如下
```
.banner{
	background-image: url(./images/rs-cover-2-2-1-1.jpg);
	height: 517px;
	background-position: center center;
	background-size: cover;
}
```
代码没有设定整个宽度，目的是可以让图片可以自适应不同屏幕的宽度，但是要设置background-position: center center，表示图片在水平方向上和垂直方向上居中，background-size: cover 表示盖住所有的面积，按比例缩放。

制作中间的大卡片
HTML代码结构
```
<div class="userCard">
	<div class="pictureAndText clearfix">
			<div class="picture"><img src="./images/icon.jpg" alt="logo"></div>
			<div class="text">
					<span class="welcome">HELLO
							<span class="triangle"></span>
					</span>
					<h1>张三</h1>
					<p>前端工程师</p>
					<hr>
					<dl class="clearfix">
							<dt>年龄</dt>
							<dd>18</dd>
							<dt>所在城市</dt>
							<dd>北京</dd>
							<dt>手机</dt>
							<dd>13812345678</dd>
							<dt>邮箱</dt>
							<dd>535435@qq.com</dd>
					</dl>
			</div>
	</div>
	<footer class="media">
			<a href=""><svg class="icon" aria-hidden="true">
					<use xlink:href="#icon-github"></use>
			</svg></a>
			<a href=""><svg class="icon" aria-hidden="true">
					<use xlink:href="#icon-weibo"></use>
			</svg></a>
			<a href=""><svg class="icon" aria-hidden="true">
					<use xlink:href="#icon-twitter"></use>
			</svg></a>
	</footer>
</div>
```
首先需要为卡片的结构进行一个划分，整个卡片一用一个大的DIV分类。根据需求初步分为上下结构，上部分是pictureAndText，下面是footer。但是需要为了达到左右布局的效果 ，在pictureAndText还需要内在进行一个div的划分，分为picture和text两个div方便进行左右的布局。
主要css代码
```
.userCard{
	max-width: 940px; 
	margin: auto;
	...
}
```
上面两句css代码实现的效果是为整个卡片设置一个最大的宽度，margin： auto; 是为了使得大卡片div水平居中，这是一个常规的套路，能使一个有宽度的div水平居中的。前提是有一个宽度。

```
.userCard .picture,
.userCard .text{
	float: left;
}
.userCard .text{
	width: 470px;
}
```

上面两句代码实现的水平左右布局，谨记在父元素上加上clearfix类。还有一点是float元素会整体收缩，如果宽度是有要求的，那就不得不为text整个div加上宽度。

```
.userCard .text dl dd,
.userCard .text dl dt {
	float: left;
	width: 50%; //关键
	margin-bottom: 25px;
}
```

上面两句主要是卫视实现了如图样式
![Alt text](https://i.loli.net/2018/05/09/5af2e973a0127.png)
这里关键在于设置宽度为width: 50%，这表示一行只能够有两个元素。实现每行只有两个元素组成一组数据。

引入ICON
在iconfont.cn选择合适的icon按照提示引入页面。

```
.userCard footer{
	background: #e8676b;
	text-align: center; //水平居中
}
.userCard footer svg{
	fill: white;       // 改变icon填充的颜色
	width: 30px;       // 改变icon大小
	height: 30px;      // 宽高许一同设置才有效果
	vertical-align: top;  //icon垂直居中
}
.userCard footer a{
	display: inline-block;   //改为inline-block包裹住里面的svg
	border: 1px transparent solid;
	border-radius: 50%;
	padding: 4px;
	margin: 0 15px;
}
.userCard footer a:hover{
	background:#cf5d5f;
}
```


