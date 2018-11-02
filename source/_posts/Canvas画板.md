---
title: Canvas画板
date: 2018-6-4 10:42:58
tags:
---

## 使用canvas实现一个画板

canvas的基本用法
首先在HTML
```
<canvas id="canvas" width="150" height="150"></canvas>
```
不要使用CSS来试图控制canvas的宽高。
基本用法
```
var canvas = document.getElementById('canvas');
var ctx = canvas.getContext('2d');
//接下来画个矩形

ctx.fillStyle = "rgb(200,0,0)";  //填充的颜色
ctx.fillRect (10, 10, 55, 50);  //准确来说是填充一个矩形
```
fillRect(x，y，width,height)接受四个参数：x与y指定了在canvas画布上所绘制的矩形的左上角（相对于原点）的坐标。width和height设置矩形的尺寸。
strokeRect(x, y, width, height) 绘制一个矩形的边框
clearRect(x, y, width, height) 清除一个矩形

绘制一个填充三角形
```
ctx.beginPath(); //必须
ctx.moveTo(75, 50);   //起点
ctx.lineTo(100, 75);  //第二个点
ctx.lineTo(100, 25);	//终点
ctx.fill();     //闭合
```
绘制一个描边三角形
```
 ctx.beginPath();
 ctx.moveTo(125,125);
 ctx.lineTo(125,45);
 ctx.lineTo(45,125);
 ctx.closePath();  //闭合
 ctx.stroke();
```
moveTo(x, y)将笔触移动到指定的坐标x以及y上。
lineTo(x, y)绘制一条从当前位置到指定x以及y位置的直线。
画圆形或圆弧
arc(x, y, radius, startAngle, endAngle, anticlockwise)画一个以（x,y）为圆心的以radius为半径的圆弧（圆），从startAngle开始到endAngle结束，按照anticlockwise给定的方向（默认为顺时针）来生成。
这里详细介绍一下arc方法，该方法有六个参数：x,y为绘制圆弧所在圆上的圆心坐标。radius为半径。startAngle以及endAngle参数用弧度定义了开始以及结束的弧度。这些都是以x轴为基准。参数anticlockwise为一个布尔值。为true时，是逆时针方向，否则顺时针方向。
注意：arc()函数中表示角的单位是弧度，不是角度。角度与弧度的js表达式: 弧度=(Math.PI/180)*角度。

```
ctx.beginPath();
 var radius = 20; // 圆弧半径
 var startAngle = 0; // 开始点
 var endAngle = Math.PI*2; // 结束点
 var anticlockwise = false；
 var x = 25；
 var y = 25；
 ctx.arc(x, y, radius, startAngle, endAngle, anticlockwise);
 ctx.fill(); //或者使用ctx.stroke() 描边
```

实现思路
监听三个鼠标事件，分别是onmousedown，onmousemove，onmouseup。当用户按下鼠标，通过canvas的API画下一个点
```
canvas.onmousedown = function(event){
	var x = event.clientX
	var y = event.clientY   //获取用户按下鼠标是的坐标
	ctx.beginPath();
	ctx.arc(x,y,3,0,Math.PI*2);   //画一个半径为3像素的圆
	ctx.fill();
}
```
监听mousemove事件，但是不画圆，画线。划线需要两个点，当用户按下鼠标的是起点，第二个点是当前瞬间mousemove的点。所以需要一个变量记录last point
```
var lastPoint = {x: 0,y:0}
canvas.onmousedown = function(event){
	var x = event.clientX
	var y = event.clientY   //获取用户按下鼠标是的坐标
	lastPoint = {x:x,y:y}       
	ctx.beginPath();
	ctx.arc(x,y,3,0,Math.PI*2);   //画一个半径为3像素的圆
	ctx.fill();
}
canvas.onmousedown = function(event){
	var x = event.clientX
	var y = event.clientY
	var newPoint = {x: x,y: y}
	ctx.beginPath();
	ctx.lineWidth = 6;  //线的宽度
	ctx.moveTo(lastPoint.x,lastPoint.y);
	ctx.lineTo(newPoint.x,newPoint.y);
	ctx.closePath();
	ctx.stroke();
	lastPoint = newPoint //importent
}
```
实际上还需要一个变量来记录当前的状态，来记录用户是否按下鼠标或者松开鼠标，否则mousemove一直在触发，一直在画线。
```
var canvas = document.getElementById('canvas');
var eraser = document.getElementById('eraser');
var ctx = canvas.getContext('2d');
getSize();
window.onresize = function() {
    getSize();
}
function getSize () {
	var pageWidth = document.documentElement.clientWidth
	var pageHeight = document.documentElement.clientHeight
	canvas.width = pageWidth
	canvas.height = pageHeight
}
实现一个简易画板
功能包括此下载，删除，橡皮擦，不同颜色的画笔。
下载功能代码
```
download.onclick = function () {
	var image = canvas.toDataURL("image/png").replace("image/png", "image/octet-stream")
	window.location.href=image;
}
```
特性检测
画板为了实现在PC端移动端兼容，监听的事件是不一样的，所以需要判断一下当前用户的设备是否支持触摸。
```
document.body.ontouchstart !== undefined
```
当ontouchstart事件为undefined是表明当前设备不支持触摸，我们监听的是mouse事件。
另外为了兼容移动端的设备，css加上meta vp ，保证页面不会被缩放。
当用户设备支持touch事件，监听的是ontouchstart，ontouchmove，ontouchend事件，这几个事件也鼠标事件稍微有一些不一样。
当触发鼠标事件的时候，我们通过clientX和clientY获取到坐标信息，touch事件获取用户坐标是touches[0].clientX和touches[0].clientY。