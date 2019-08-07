---
title: '[js] 多次点击触发相应操作'
date: 2018-08-14 20:04:40
tags: js
---

 有段时间没写 Android 了，最近公司业务重心转移到小程序上了，我也随之改变写起了小程序，由于以前写过一点点点点点 React Native，刚转到小程序也没有太大的不适应，反正都是要自己边学边做，由于没有前端基础，只好在业务中发现问题，然后查文档补基础。不过说回来前端的门槛确实不高，很容易入门。

最近 PM 要求给小程序加一个隐藏的测试开关，开启之后可以看到测试数据，这个功能仅限内部使用，我第一反应就是在指定的极短的时间内点击某处 n 次，触发这个开关（以前写 Android 埋的开关都是这样埋的😂），上代码：

```js
function multiClick(fn, times = 8, duration = 2000) {
  let timeStamps = []
  return function() {
    let timeStamp = arguments[arguments.length - 1].timeStamp
    timeStamps.push(timeStamp)
    if (timeStamps.length === times) {
      if (timeStamps[times - 1] - timeStamps[0] < duration) {
        fn.apply(this, arguments)
      }
      timeStamps = []
    }
  }
}
```

在小程序中，事件的 event 参数是在参数列表最后一个，所以使用 `arguments[arguments.length - 1].timeStamp`  获取 event 中触发该次事件时的 timeStamp（ps：这个 timeStamp 是从小程序启动到触发该次事件的间隔时间）。

![](console.png)

不过上面的代码没有必要存时间数组，懒得改了。在使用的地方：

```js
methods = {
  openDebug: utils.multiClick(function () {
    // TODO something
  })
}
```

上面的方式使用到了 [arguments](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/arguments) 和 [apply](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)，其实我之前完全不知道这两个东西的，看到了项目中有一个‘防止快速点击’的函数：

```js
function throttle(fn, gapTime = 1500) {
  let _lastTime = null
  return function() {
    let _nowTime = +new Date()
    if (_nowTime - _lastTime > gapTime || !_lastTime) {
      fn.apply(this, arguments)
      _lastTime = _nowTime
    }
  }
}
```

然后查了一下文档才了解。

当然，也可以用类来实现，这种方式就比较简单了：

```js
let timeStamps = [];
export default class ClickUtils {
  static multiClick(callback, times = 8, duration = 2000) {
    let timeStamp = Date.now();
    timeStamps.push(timeStamp);
    if (timeStamps.length === times) {
      if (timeStamps[times - 1] - timeStamps[0] < duration) {
        callback();
      }
      timeStamps = [];
    }
  }
}
```

```js
methods = {
  openDebug () {
    ClickUtils.multiClick(function () {
      // TODO
    })
  }
}
```

