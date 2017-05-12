## deferred对象
> deferred对象就是jQuery的回调函数解决方案

`javascript`中有一些操作比较耗时，如ajax操作，面对这样的情况，我们通常会指定回调函数。所谓回调函数就当这些操作结束后，应该调用哪些函数。

---

#####一、传统的ajax操作
```javascript
$.ajax({
    url : index.html,
    success : function(){
        alert('success');
    },
    error : function(){
        alert('fail');
    }  
});
```
这里的`success`方法指定了操作成功后的回调函数，`fail`方法指定了操作失败后的回调函数。下面我们再来看一下，用`deferred`对象，写法是怎样的？

---

#####二、deferred对象的链式操作

```javascript
$.ajax('index.html')
.done(function(){alert('success');})
.fail(function(){alert('fail');});
```
这里的`done()`相当于`success`方法，`fail()`相当于`error`方法。这种链式写法很清晰、可读性也很强。deferred对象的强大还不止于此。

**1、deferred对象可以为一个操作指定多个回调函数。**


```javascript
$.ajax('index.html')
.done(function(){alert('success');})
.fail(function(){alert('fail');})
.done(function(){alert('第二个回调函数');});
```
像上面这样，我们可以指定任意多个回调函数，这些回调函数按顺序执行。


**2、为多个操作指定回调函数**

```javascript
$.when($.ajax('index1.html'),$.ajax('index.html'))
.done(function(){alert('success');})
.fail(function(){alert('fail');});
```
 这里我们用到了`$.when()`方法，这个方法的参数是`deferred`对象，这段代码的意思是只有两个操作都成功了，才运行`done()`指定的回调函数，其他情况运行`fail()`指定的回调函数.
 
 ---

#####三、普通操作的回调函数
> deferred对象最大的有点在于，对于ajax操作、本地操作；异步操作、同步操作，都可以使用deferred对象的方法指定回调函数.

比如说有一个很耗时的操作`wait`:

```javascript
var wait=function(){
    var tasks=function(){
        alert('执行完毕');
    }
    setTimeout(tasks,5000);//真的很耗时
}
```
如果我们想为这个函数指定回调函数，应该怎么做呢？我们首先来深入的了解一下`deferred`对象.

**1、三种执行状态**

`deferred`对象有三种执行状态：未完成、已完成、已失败。如果状态是已完成，`deferred`对象就是调用`done()`方法指定的回调函数；如果状态是已失败，`deferred`对象就是调用`fail()`方法指定的回调函数。`deferred.resolve()`方法将执行状态从未完成改为已完成，从而调用`done()`方法；`deferred.reject()`方法将执行状态从未完成改为已失败，从而调用`fail()`方法。

**2、改变状态**

前面提到的`ajax`操作，`deferred`对象会根据返回的结果，自动改变自身的状态；但在`wait`函数中，需要手动改变 `deferred`的执行状态，从而触发相关回调函数。

**3、`deferred.promise()`方法**

`deferred.promise()`的作用是在原来`deferred`对象上返回另一个`deferred`对象，后者只开放与执行状态无关的方法(如 `done()` 方法和 `fail()` 方法)，屏蔽改变执行状态有关的方法(如 `resolve()`方法`reject()` 方法)。

-

了解了以上三点之后，我们来为`wait()`函数指定回调函数。

```javascript
var wait=function(){
    var dfd=$.Deferred(); //在函数内部创建一个deferred对象
    var tasks=function(){
        alert('执行完毕');
        dfd.resolve(); //改变执行状态
    }
    setTimeout(tasks,5000);
    return dfd.promise(); //返回promise对象
};

$.when(wait())
.done(function(){alert('success');})
.fail(function(){alert('fail');})
```
**这里有三点需要注意：**

* 最好在函数的内部创建`deferred`对象，这样可以防止在外部修改`deferred`对象的执行状态。
* 最好返回`promise`对象，还是防止执行状态在函数外部被改变。
* 要在合适的时候改变`deferred`对象的执行状态，一般是在耗时的操作执行完之后，改变`deferred`对象的执行状态。

这样我们就完成了为普通操作添加回调函数。

---

#####四、`deferred`对象的其他方法

`deferred.then()`方法，接受两个参数，第一个参数是`done()`方法的回调函数；第二个参数是`fail(`)方法的回调函数。具体写法如下：

```javascript
$.when($.ajax('index.html'))
.then(successFunction , failFunction);
```
这样省力了一下呢！

`deferred.resolve(arg)`方法是可以接受参数的，这个参数将会传递给通过`deferred.then`或`deferred.done` 添加的`doneCallbacks`

---
本文主要简单的介绍了一下`deferred`对象的使用场景和使用方法。

关于`deferred`对象的其他方法，请大家查看[jquery官方文档](http://www.jquery123.com/category/deferred-object/)

我在学习过程中阅读的[相关文献](http://www.ruanyifeng.com/blog/2011/08/a_detailed_explanation_of_jquery_deferred_object.html)


