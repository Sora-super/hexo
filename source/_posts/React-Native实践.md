---
title: React Native实践
date: 2021-01-12 15:59:15
tags:
---

背景：
    前领导今天突然问我："Reactive Native你玩过吗"
    我的手放在键盘良久，突然发现，我至少听过几百次Reactive Native、Reactive Native、Reactive Native，但确实没有去实践过，这不应该是一名符合24真言的社会十好青年应该干的事
    那么，去吧，皮卡丘！


前言：
    知其然，不知其所以然
    我们可以不深入，但是不可以不了解，至少在别人问你的时候，能道出个一二


Q: 什么是React Native，为什么要使用它

A: React Native是一款开源的跨平台移动应用开发框架，支持iOS和安卓两大平台，使用的语言是Javascript，非常适合前端人员。
它具有以下几点优势

1. 跨平台兼容性
2. 相较于其他跨平台开发框架拥有卓越的性能
3. 强大的生态
4. javascript语言，较低的学习成本
5. 热更新！
> 频繁的升级app必然会让用户烦躁，过多的业务迭代带来的app审核，安卓还好点，ios审核更是app开发者的噩梦



开始使用它！

##  [搭建开发环境](https://reactnative.cn/docs/environment-setup)

必须安装的依赖有：Node、Watchman、Xcode 和 CocoaPods。

官网有个⚠️，🚫使用cnpm安装

```
brew install node
brew install watchman
```

安装完node以后，使用淘宝镜像源加速

```
# 使用nrm工具切换淘宝源
npx nrm use taobao

# 如果之后需要切换回官方源可使用
npx nrm use npm
```


安装Xcode，直接在appstore中搜索
这个安装的很慢，网速不好的可以放弃了


安装cocoapods
cocoapods包管理工具【可以理解为ios的npm】

```
brew install cocoapods
```


## 创建新项目

使用React Native内建的命令行工具创建一个名为AwesomeProject的项目

```
npx react-native init AwesomeProject
```

安装依赖

```
yarn ios
```

## 大功告成

恭喜！你已经成功运行并修改了你的第一个 React Native 应用。

