---
title: Vue源码解析（三）
date: 2017-09-04 22:24:13
tags: 前端
---
接下来调用eventsMixin方法，看名称就知道是关于事件相关的，主要是在Vue的原型上加入`$on,$once,$off,$emit`方法。

<!-- more -->

```
Vue.prototype.$on = function (event, fn) {
    var this$1 = this;

    var vm = this;
    if (Array.isArray(event)) {
      for (var i = 0, l = event.length; i < l; i++) {
        this$1.$on(event[i], fn);
      }
    } else {
      (vm._events[event] || (vm._events[event] = [])).push(fn);
      // optimize hook:event cost by using a boolean flag marked at registration
      // instead of a hash lookup
      if (hookRE.test(event)) {
        vm._hasHookEvent = true;
      }
    }
    return vm
  };
```
`$on`方法为监听自定义事件，如果event为数组，则遍历event数组进行监听。如果为字符串则放入vm._events数组中。这块还进行了一个事件是否为Hook的正则判断，暂未搞清楚具体作用。
```
Vue.prototype.$off = function (event, fn) {
    var this$1 = this;

    var vm = this;
    // all
    if (!arguments.length) {
      vm._events = Object.create(null);
      return vm
    }
    // array of events
    if (Array.isArray(event)) {
      for (var i$1 = 0, l = event.length; i$1 < l; i$1++) {
        this$1.$off(event[i$1], fn);
      }
      return vm
    }
    // specific event
    var cbs = vm._events[event];
    if (!cbs) {
      return vm
    }
    if (arguments.length === 1) {
      vm._events[event] = null;
      return vm
    }
    // specific handler
    var cb;
    var i = cbs.length;
    while (i--) {
      cb = cbs[i];
      if (cb === fn || cb.fn === fn) {
        cbs.splice(i, 1);
        break
      }
    }
    return vm
  };
```
`$off`方法为解除已绑定的事件，如果off方法不传任何参数，删除所有绑定事件。如果event为数组，则循环解除数组内的事件。如果为字符串，事件数组不存在则直接返回；如果不需要回调，则将相关事件数组设为null。如果该事件数组有存在，则遍历事件数组，并从数组中删除符合条件的事件。
```
Vue.prototype.$once = function (event, fn) {
    var vm = this;
    function on () {
      vm.$off(event, on);
      fn.apply(vm, arguments);
    }
    on.fn = fn;
    vm.$on(event, on);
    return vm
  };
```
`$once`方法为绑定仅响应一次事件，响应结束后立即解除绑定。主要是调用了`$on`方法，同时回调时调用`$off方法`解除该事件，并再解除事件后进行回调。
```
Vue.prototype.$emit = function (event) {
    var vm = this;
    {
      var lowerCaseEvent = event.toLowerCase();
      if (lowerCaseEvent !== event && vm._events[lowerCaseEvent]) {
        tip(
          "Event \"" + lowerCaseEvent + "\" is emitted in component " +
          (formatComponentName(vm)) + " but the handler is registered for \"" + event + "\". " +
          "Note that HTML attributes are case-insensitive and you cannot use " +
          "v-on to listen to camelCase events when using in-DOM templates. " +
          "You should probably use \"" + (hyphenate(event)) + "\" instead of \"" + event + "\"."
        );
      }
    }
    var cbs = vm._events[event];
    if (cbs) {
      cbs = cbs.length > 1 ? toArray(cbs) : cbs;
      var args = toArray(arguments, 1);
      for (var i = 0, l = cbs.length; i < l; i++) {
        cbs[i].apply(vm, args);
      }
    }
    return vm
  };
```
`$emit`方法为事件下发的方法，首先进行了一个大小写的判断，如果注册事件和下发事件的大小写不一致，则发出警告。如果该事件数组存在，则遍历事件数组进行注册回调，同时使用apply方法传递调用时参数。