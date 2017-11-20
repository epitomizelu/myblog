---
title: firstblog
date: 2017-11-19 23:32:40
tags:
---

#  一，为什么要函数节流
> 在前端开发中，要注意的是，dom操作耗费的资源比非dom操作要昂贵的多，如果频繁的进行dom操作，有可能会让浏览器崩溃。比如onresize事件，当改变浏览器窗口的大小的过程中，该事件不止一次的运行，会连续调用多次，如果在该事件的回调函数中包含dom操作，会严重影响性能。在类似这种情况下，就需要进行函数节流。


# 二，函数节流的原理及其实现

> 函数节流的原理用一句话来概括就是：最小事件间隔内不允许重复调用函数。
也就是说，设置setTimeOut，setTimeOut的回调就是你要执行的代码，如果该你指定的时间间隔内，重复调用的话，就将之前设置的定时任务取消掉。代码如下：
```javascript
function processor(method,context) {
    clearTimeout(method.id);
    method.id = setTimeout(function() {
          method.call(context);
    },500);
}
```
> 另有一种写法
```javascript
function throttle(method,expire,cxt) {
    var timer = null;
    return function() {
        clearTimeout(timer);
        setTimeout(function() {
             method.call(cxt);
        },expire);
   }
}
```

> 这两种并没有大的区别，第二种使用了闭包，把所有变量都封装到闭包了。我认为还是第一种写法比较简洁。


# 三，深入思考
> 第二部分的思路和写法也是有缺陷的，比如一种极端情况：长时间的调同一个函数，直到其返回结果才停止调用。基于第二部分的代码写出下面这个例子：
```javascript
var stop = false；
function callback() {
    stop = true;
}

var handler = throttle(callback,500);

while(!stop) {
    handler();
}
```

> 你会发现，在上面这种使用场景下，会陷入死循环，callback永远不会被调用。要解决这个问题，可以这样想，指定一个时间间隔，在这个时间间隔内不调用函数，当达到这个时间间隔，就调用一次函数。相当于使用setInterval。但并不直接使用setInterval，而是用setTimeout去模拟。
```javascript
function throttle(method,expire,cxt) {
    var timer = null,
    now = new Date();
    return function() {
        if(new Date() - now > expire) {
            method.call(cxt);
            now = new Date();
            clearTimeout(timer);
        }
         clearTimeout(timer);
        setTimeout(function() {
             method.call(cxt);
        },expire);
   }
}

```




