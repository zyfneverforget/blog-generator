---
title: Cookie
date: 2018-8-30 09:36:53
tags:
---

# Cookie Session Localstorage 
## Cookie 和 Session 的区别
因为HTTP是无状态的协议，当需要记录用户的信息的时候可以通过Cookie实现。
Cookie是服务器通过 Set-Cookie 头给客户端一串字符串，这个就是Cookie，客户端每次访问相同域名的网页时，必须带上这段字符串。而且Cookie保存对应用户的相关信息。客户端要在一段时间内保存这个Cookie。但是Cookie不够安全，用户能够直接修改Cookie的信息。为了解决安全的问题，我们引入Session。
Session是将 SessionID（随机数）通过 Cookie 发给客户端，客户端访问服务器时，服务器读取 SessionID，服务器有一块内存保存了所有 session，通过 SessionID 我们可以得到对应用户的隐私信息。因为是SessionID随机数，用户就无法修改得到想要的信息。
node.js部分代码：
```
let sessionId = Math.random() * 100000
response.setHeader('Set-Cookie', `sessionId=${sessionId}`)
```
## Cookie 和 LocalStorage 的区别
Cookie 是HTTP 协议里的内容。LocalStrorage与HTTP协议没有关系，只是HTML5新提供的一个API所以HTTP请求或响应都 不会带上 LocalStorage 的值。还有一点是Cookie是会过期的，但是LocalStorage 永久有效，除非用户清理缓存。
而且跟Cookie一样，只有相同域名的页面才能读取LocalStorage，
##LocalStorage 和 SessionStorage 的区别
LocalStorahe和SessiomStrorage区别是LoalStroage里的数据信息是一直存在的，只要用户不删除缓存，就像我们存在电脑硬盘上存储的数据一样。但是SessionStroage不一样，当浏览器或者页面关闭了SessionStroage就会被清空。
## Cookie 如何设置过期时间？如何删除 Cookie？
有两种方法一种是在服务器段写上```Set-Cookie: id=xxx; Expires=Wed, 21 Oct 2018 07:28:00 GMT```代码内Expires的值就是这个Cookie的过期时间，需要注意的是时间的格式是格林威治时间。
还有一种是```Set-Cookie: id=xxx; Max-Age=23456787```这个设置的是在 cookie 失效之前需要经过的秒数。接受一位或多位非零（1-9）数字。
如果在浏览器上删除Cookie，可以直接在浏览器的设置选项来进行操作。如果是使用JS来清楚Cookie的话可以通过API
docCookies.removeItem(name[, path],domain)来清除Cookie。
## Cache-Control: max-age=1000 缓存 与 ETag 的「缓存」有什么区别？
Cache-Control可以用来控制缓存，当设置了Cache-Control: max-age=1000，等于设置缓存存储的最大周期为1000秒，只要还在这个时间内，页面在请求相同URL的资源，浏览器就不会发送请求，直接读取上一次请求响应的缓存。当需要更新缓存，可以更新文件的查询参数，这样使得URL发生改变，更新新的文件。
当我们使用ETag来进行缓存控制，一般做法是比较的是两次请求返回的ETag的MD5的值，如果这两次请求返回的ETag的值相同，则不下载。但是浏览器还是会发请求的，服务器也有响应，但是响应的第四部分是空的。
两种最主要的区别就是Cache-Control缓存存储的有效周期内不会发请求相同URL的资源不会发请求，但是使用ETag浏览器还是会发请求。
