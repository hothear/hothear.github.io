---
title: JS的防抖与节流
tags: 前端
cover: https://www.elle.com.hk/var/ellehk/storage/images/celebrity/feature/which-actors-starting-as-joker-beside-joaquin-phoenix-in-history/node_1762362/31159499-1-chi-HK/01_img_885_590.jpg
---
项目中经常需要处理防止用户多次提交表单的情况，简单的方法就是设定`status`变量当ajax得到返回之后再将`status=true`，在体量比较小的项目中，这么处理还算可以，但是项目如果有大量的用户点击请求操作的话每次加状态判断显得很繁琐，这时我们可以封装统一的节流函数。

## 节流函数（throttle）
### 概念
规定一个单位时间，在这个单位时间内，只能有一次触发事件的回调函数执行，如果在同一个单位时间内某事件被触发多次，只有一次能生效。
### 应用场景
用户表单提交/滚动条加载
### 代码
``` javascript
function throttle(fn, gapTime) {
  let _lastTime = null;

  return function () {
    let _nowTime = + new Date()
    if (_nowTime - _lastTime > gapTime || !_lastTime) {
      fn();
      _lastTime = _nowTime
    }
  }
}

let fn = ()=>{
  console.log('boom')
}

setInterval(throttle(fn,3000),1000)

```
## 防抖函数（debounce）
### 概念
在事件被触发n秒后再执行回调，如果在这n秒内又被触发，则重新计时。
### 应用场景
- search搜索联想，用户在不断输入值时，用防抖来节约请求资源
- window触发resize的时候，不断的调整浏览器窗口大小会不断的触发这个事件，用防抖来让其只触发一次
### 代码
``` javascript
function debounce(fn, wait) {
  var timer = null;
  return function () {
      var context = this
      var args = arguments
      if (timer) {
          clearTimeout(timer);
          timer = null;
      }
      timer = setTimeout(function () {
          fn.apply(context, args)
      }, wait)
  }
}

var fn = function () {
  console.log('boom')
}

setInterval(debounce(fn,500),1000) // 第一次在1500ms后触发，之后每1000ms触发一次

setInterval(debounce(fn,2000),1000) // 不会触发一次（我把函数防抖看出技能读条，如果读条没完成就用技能，便会失败而且重新读条）
```