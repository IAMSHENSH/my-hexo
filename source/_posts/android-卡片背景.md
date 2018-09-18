---
title: Android 卡片背景
date: 2017-09-27
tags: 
- android
- card
- xml
- note
categories: Android
---

> 关于 Android 的 卡片背景制定

<!-- more -->

# 背景

- 因为 CardView 的卡片形式可用于许多场景。但是 v7 CardView 的 jar 包与 aar 包引入项目中总会出错，暂时找不到解决方案，所以就干脆直接编写 XML 文件来制作一张类似于 CardView 的万能背景。

# 要素

- 在此卡片背景中，需要在 Android 项目的「drawable」中新建一个 xml，用于编写背景，而被引用。
- 会用上的元素包括：`layer-list`,`item`,`shape`

# 定义

在「drawable」中创建 `bg_card.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">

    <item
        android:left="@dimen/card_shadow"
        android:top="@dimen/card_shadow">
        <shape android:shape="rectangle">
            <solid android:color="@color/card_white_shadow" />
            <corners android:radius="@dimen/card_radius" />
        </shape>
    </item>

    <item
        android:bottom="@dimen/card_shadow"
        android:right="@dimen/card_shadow">
        <shape android:shape="rectangle">
            <solid android:color="@color/card_white_bg" />
            <corners android:radius="@dimen/card_radius" />
        </shape>
    </item>

</layer-list>
```

> - `layer-list` 就是将内部多个元素同时暂时在画面上的标签
> - 这里用到两个`item`，上面的为阴影效果，下面的为背景效果
> - 其中 `android:left`，`android:top`与`android:bottom`,`android:right`分别控制两个 `item`的错位效果
> - `shape` 标签用于绘制各种形状，`android:shape="rectangle`用于表示绘制矩形，不过好像不写这个也有效果，待测试验证。
> - `solid` 表示图像填充的颜色
> - `corners` 表示图像的圆角角度

# 使用

在其他文件中，只要通过设置其背景为此 xml 即可。

```xml
<RelativeLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@drawable/bg_card">
</RelativeLayout>
```

---