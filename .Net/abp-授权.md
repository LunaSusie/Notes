---
title: abp-授权
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

# 授权
## 介绍
几乎所有的企业应用程序都使用某种级别的授权。授权用于检查是否允许用户在应用程序中执行某些特定的操作。
## 关于IPermissionChecker
授权系统使用IPermissionChecker来检查权限。
虽然你可以用你自己的方式来实现它，但它完全在 模块零项目中实现。
如果没有实现，则使用NullPermissionChecker，将所有权限授予每个人。
## 定义权限