---
title: Promise
date: 2018-10-31 11:12:12
tags:
---


Promise写法
```
let test = function (a) {
	return new Promise(function(resolve,reject){
		if(a){
			reslove('这个参数传递给处理成功的回调函数')
		} else {
			reject('这个参数传递给处理失败的函数')
		}
	})
}

test(a).then((x)=>{//处理成功函数},(y)=>{//处理失败函数})
```
构造函数Promise接受一个函数作为参数，这个函数必须有两个参数，一个是resolve，一个是reject。一般成功的时候调用resolve函数，失败时调用reject函数。
Promise链
当有多个Promise链式调用时
```
let test = function (a) {
	return new Promise(function(resolve,reject){
		if(a){
			reslove('这个参数传递给处理成功的回调函数')
		} else {
			reject('这个参数传递给处理失败的函数')
		}
	})
}

function b(string){
	return new Promise((resolve,reject)={
		if(string==='这个参数传递给处理成功的回调函数'){
			reslove(//传递给下一个成功处理函数)
		}
		reject(//传递给下一个失败处理函数) 
	})
}
function c(string){
	return new Promise((resolve,reject)={
		reject(//传递给下一个失败处理函数) 
	})
}
function d(string){
	return new Promise((resolve,reject)={
		resolve(//传递给下一个成功处理函数) 
	})
}
function e(string){
	return new Promise((resolve,reject)={
		reject(//传递给下一个失败处理函数) 
	})
}
test(a).then(b,c).then(d,e)
```
##链式调用执行顺序
当test函数执行了resolve函数，下一步会执行then里面的第一个参数函数，也就是函数b。当test函数执行了reject函数，下一步会执行then里面的第二个参数函数，也就是函数c。需要注意的是，函数b也返回一个Promise对象，当函数b执行了resolve函数，下一步会执行函数d，当函数b执行了reject函数，下一步会执行函数e,到这里都比较好理解。但是当函数c执行的是resolve，那下一步也会执行函数d。如果要函数e被执行，则必须函数b或者函数c其中一个被执行，执行的顺序不是当函数c被执行，函数e就会被执行。
##Promise.all()
Promise.all(iterable)方法返回一个Promise实例，此实例在iterable参数内所有的promise都“完成（resolved）”或参数中不包含 promise 时回调完成（resolve）；如果参数中promise 有一个失败（rejected），此实例回调失败（reject），失败原因的是第一个失败 promise 的结果。具体参考MDN
