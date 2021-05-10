---
title: chrome输入框自动填充问题
category: 实战
tag:
  - chrome
  - 输入框
comment: false
---

# 前言
当我们在做用户登录时，chrome会提示我们是否要保存密码，这时如果我们点击保存的话，在其他页面就有可能出现问题。

在我们进入到包含密码输入框的页面后，浏览器会将保存的密码自动填充到输入框，无论这个值是否是我们预期想要的，甚至还会将保存的账号信息填充到在密码输入框之上的输入框中。

# 查找解决方案
## 方案一 生成隐藏虚拟input让内容填充到该输入框中

直接否定，生成无用元素或多或少对性能是有影响的

## 方案二 使用autocomplete="off"

经过测验无效

## 方案三 使用autocomplete="new-password"
经测验有效

> 如果你定义了一个用户管理页面，其中用户可以为其他人指定新的密码，因此你想阻止密码字段的自动填充，你可以使用 autocomplete="new-password"。

> 这只是一个提示，浏览器不一定要遵守。但现代浏览器都已停止在设置了 autocomplete="new-password" 的 <input> 元素上使用自动填充

引用于 [如何关闭表单自动填充](https://developer.mozilla.org/zh-CN/docs/Web/Security/Securing_your_site/Turning_off_form_autocompletion)