---
title: Android Selector
date: 2017-09-27
tags: 
- android
- sekector
- xml
- tools
categories: Android
---


> 关于 Android 的 Selector

<!-- more -->

# 背景

- `selector` 标签是在安卓工程的 XML 文件定义的资源文件。用于提高软件的交互体验。
- 例如：在 「Button」 被按下时，选中与未选中的状态，即可由 `selector` 中设定。

# 属性

- `state_enabled`: 设置触摸或点击事件是否可用状态，一般只在 `false` 时设置该属性，表示不可用状态
- `state_pressed`: 设置是否按压状态，一般在 `true` 时设置该属性，表示已按压状态，默认为 `false` 
- `state_selected`: 设置是否选中状态，`true` 表示已选中，`false` 表示未选中
- `state_checked`: 设置是否勾选状态，主要用于 `CheckBox` 和 `RadioButton`，`true` 表示已被勾选，`false` 表示未被勾选
- `state_checkable`: 设置勾选是否可用状态，类似`state_enabled`，只是`state_enabled`会影响触摸或点击事件，而state_checkable影响勾选事件
- `state_focused`: 设置是否获得焦点状态，`true`表示获得焦点，默认为`false`，表示未获得焦点
- `state_window_focused`: 设置当前窗口是否获得焦点状态，`true` 表示获得焦点，`false` 表示未获得焦点，例如拉下通知栏或弹出对话框时，当前界面就会失去焦点；另外，`ListView` 的`ListItem` 获得焦点时也会触发 `true` 状态，可以理解为当前窗口就是 `ListItem`本身
- `state_activated`: 设置是否被激活状态，`true` 表示被激活，`false` 表示未激活，「API Level 11」及以上才支持，可通过代码调用控件的 `setActivated(boolean)`方法设置是否激活该控件
- `state_hovered`: 设置是否鼠标在上面滑动的状态，`true` 表示鼠标在上面滑动，默认为`false`，「API Level 14」及以上才支持

# 例子

样例代码：

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <!-- 当前窗口失去焦点时 -->
    <item android:color="@android:color/black" android:state_window_focused="false" />
    <!-- 不可用时 -->
    <item android:color="@android:color/background_light" android:state_enabled="false" />
    <!-- 按压时 -->
    <item android:color="@android:color/holo_blue_light" android:state_pressed="true" />
    <!-- 被选中时 -->
    <item android:color="@android:color/holo_green_dark" android:state_selected="true" />
    <!-- 被激活时 -->
    <item android:color="@android:color/holo_green_light" android:state_activated="true" />
    <!-- 默认时 -->
    <item android:color="@android:color/white" />
</selector>
```

# 注意

- 一定要注意，在 `selector` 内部的 `item` 顺序问题，不同的顺序会造成不同的效果。有时候设置的效果没有生效，往往是顺序的问题。
- item是从上往下匹配的，如果匹配到一个item那它就将采用这个item，而不是采用最佳匹配的规则；所以设置默认的状态，一定要写在最后，如果写在前面，则后面所有的item都不会起作用了。

---
