---
title: 实现类似jQuery的函数思路
date: 2018-04-16 14:17:15
tags:
---
# 实现一个 jQuery 的 API

```
window.jQuery = function(selector){ 
  let nodes = {length: 0}
  let a = document.querySelectorAll(selector)
  for(let i=0;i<a.length;i++){
    nodes[nodes.length]=a[i]
    nodes.length =nodes.length+ 1
  }
  nodes.addClass = function(className){
    for(let i=0;i<nodes.length;i++){
      nodes[i].classList.add(className)
    }
  }
  nodes.setText = function (text){
    for(let i=0;i<nodes.length;i++){
      nodes[i].textContent = text
    }
  }
  return nodes
}
window.$ = jQuery
$div.addClass('red') // 可将所有 div 的 class 添加一个 red
$div.setText('hi') // 可将所有 div 的 textContent 变为 hi
```
以上面是代码。

# 思路
jQuery是目前使用最广泛的javascript函数库。简单的理解可以说jQuery是一个函数，接受一个节点或者选择器，返回一个对象，和对象可用的新的API。
首先为了方便使用，需要给jQuery一个命名空间，所以用全局window.jQuery存放jQuery函数。
函数接受一个节点或者选择器，通过节点或选择器获取到一个或多个对象。实现的方法先生成一个新的对象，接着通过DOM的 API document.querySelectorAll，把得到的值是遍历，写入新建的对象。然后写一个方法操作这个对象，如上面的代码addClass，目的是接受一个 classNmae，可将所有 div 的 class 添加一个 red 。方法并不复杂，遍历这个伪数组，使用 DOM 的 API.classList.add(className)为每个元素加上 classNmae 便可实现。第二个方法实现的思路也是类似的。最后jQuery函数返回一个新的对象，和新的API。

