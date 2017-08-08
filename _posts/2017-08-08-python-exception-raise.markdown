---
layout: post
title:  "Welcome to Jekyll!"
date:   2017-08-08 15:39:00
categories: python exception
tags: python exception
---

# `python` 异常处理中`raise`和`raise from None`的区别

最近想把`python 3`的`selectors`模块迁移到`python 2`，于是直接将它的源码拷下来修改。突然发现里面很多用到了`python 2`不兼容的`raise from None`。

咦？如果说不想要`__cause__`的话，直接`raise`不就好了？为啥要画蛇添足，多此一举？于是抱着疑问查询了[stackoverflow](https://stackoverflow.com/questions/24752395/python-raise-from-usage/24752607#24752607)，终于找到了答案。原来`raise`也是分情况的。如果你是直接`raise`，一个`__context__`属性就不会设置。而如果你是如下场景：

```python
try:
    do_something()
except SomeError:
    raise NewError()
```

也就是说，是在异常处理中做了`raise`，则抛出来的异常中的`__context__`属性会被设置。显示这个异常的异常信息时，就会有`during handling something else happened`这个消息出现了。

而如果你想彻底取消异常的上下文信息，就需要使用`raise from None`。
