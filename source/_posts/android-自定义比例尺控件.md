---
title: Android 自定义地图比例尺控件
date: 2017-09-22
tags: 
- Android
- 自定义控件
- 比例尺
- note
categories: Android
---

![地图比例尺示意图](http://ovlnfs1rj.bkt.clouddn.com/android-scale-title.jpg)

> 关于在 Android 自定义地图比例尺控件的背景与过程

<!-- more -->

# 背景

比例尺是标识图上一条线段的长度与地面上相应线段的实际长度之比。
因此地图上的比例尺包含两个要素，线段与值。

这里我们需要自己绘制出一个比例尺控件。

# 比例尺表示选择

目前比例尺有两种表达方案：

1. 固定线段长度，改变值
2. 固定值，改变线段长度

参考目前流行的百度地图、高德地图，采用第二种方案。即在地图缩放的过程中，实施改变比例尺的长度。

# 步骤

## 1. 创建一个比例尺控件「MyScaleView.java」

## 2. 继承 View ，重写其构造方法，定义变量

```java
    Context mContext;
    int mScaleWidth;
    int mScaleHight;
    String mScaleText;
    int mTextColor;
    int mScaleSpaceText;
    int mTextSize;
    Paint mPaint;

    public MyScaleView(Context context) {
        super(context);
        this.mContext = context;
        initScale();
    }
    public MyScaleView(Context context, AttributeSet attrs) {
        super(context, attrs);
        this.mContext = context;
        initScale();
    }
    public MyScaleView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        this.mContext = context;
        initScale();
    }
```

> 其中 :
> 1. mScaleWidth 是比例尺的宽度
> 1. mScaleHight 是比例尺的高度
> 1. mScaleText 是比例尺显示的文字
> 1. mTextColor 是比例尺显示文字的颜色
> 1. mScaleSpaceText 是比例尺文字与图例的空隙
> 1. mTextSize 是比例尺文字的大小

## 3. 编写初始化参数的方法 `initScale()`，在构造方法中调用

```java
    /**
     * 初始化比例尺参数
     */
    private void initScale() {
        mScaleWidth = 120;
        mScaleHight = 10;
        mScaleText = "比例尺";
        mTextColor = Color.BLUE;
        mScaleSpaceText = 6;
        mTextSize = getResources().getDimensionPixelSize(R.dimen.txt_map_scale_size);
        mPaint = new Paint();
    }
```

> 其中`R.dimen.txt_map_scale_size`是「dimens.xml」中设置的比例尺文字大小，目的是适配不用尺寸的设备。

## 4. 编写 获取宽度，获取高度的方法，`getWidthSize()`，`getHightSize()` 

```java
    private int getWidthSize(int widthMeasureSpec) {
        return MeasureSpec.getSize(widthMeasureSpec);
    }

    private int getHeightSize(int heightMeasureSpec) {
        int mode = MeasureSpec.getMode(heightMeasureSpec);
        int height = 0;
        switch (mode) {
            case MeasureSpec.AT_MOST:
                height = mTextSize + mScaleSpaceText + mScaleHight;
                break;
            case MeasureSpec.UNSPECIFIED:
                height = Math.max(mTextSize + mScaleSpaceText + mScaleHight, MeasureSpec.getSize(heightMeasureSpec));
                break;
            case MeasureSpec.EXACTLY:
                height = MeasureSpec.getSize(heightMeasureSpec);
                break;
            default:
                break;
        }
        return height;
    }
```

## 5. 重写 `onMearsure()` 方法

```java
    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        int widthSize = getWidthSize(widthMeasureSpec);
        int heightSize = getHeightSize(heightMeasureSpec);
        setMeasuredDimension(widthSize, heightSize);
    }
```

> 使用 `setMeasuredDimension`将宽高返回给父View

## 6. 重写 onDraw() 方法

```java
    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        int scaleWidth = mScaleWidth;
        mPaint.setColor(mTextColor);
        mPaint.setAntiAlias(true);
        mPaint.setTextSize(mTextSize);
        mPaint.setTypeface(Typeface.DEFAULT);
        float textWidth = mPaint.measureText(mScaleText);
        canvas.drawText(mScaleText, (scaleWidth - textWidth) / 2, mTextSize, mPaint);
        Rect scaleRect = new Rect(0, mTextSize + mScaleSpaceText, scaleWidth, mTextSize + mScaleSpaceText + mScaleHight);
        drawNightPath(canvas, scaleRect);
    }
```

> ![比例尺说明图](http://ovlnfs1rj.bkt.clouddn.com/android-scaleandroid-scale.png)
> - 需要说明的是，`canvas.drawText()`设置文字的左下角为绘制的起点，如图所示

## 7. 编写比例尺刷新方法 refreshScaleView()

```java
    /**
     * 对外接口 ，刷新比例尺信息
     * @param width 比例尺的宽度，如 1.6 厘米
     * @param scaleText  比例尺要显示的文字
     */
    public void refreshScaleView(double width , String scaleText) {
        setScaleText(scaleText);
        setScaleWidth(width);
        invalidate();
    }
    private void setScaleText(String scaleText) {
        this.mScaleText = scaleText;
    }
    private void setScaleWidth(double width) {
        double ppi = getPPIofDevice();
        mScaleWidth = (int) (width * ppi / 2.54);
    }
    /**
     * @return 设备 PPI 值
     */
    private double getPPIofDevice() {
        Point point = new Point();
        Activity activity = (Activity) mContext;
        activity.getWindowManager().getDefaultDisplay().getRealSize(point);
        DisplayMetrics dm = getResources().getDisplayMetrics();
        double x = Math.pow(point.x / dm.xdpi, 2);
        double y = Math.pow(point.y / dm.ydpi, 2);
        double screenInches = Math.sqrt(x + y);
        return Math.sqrt(Math.pow(point.x, 2) + Math.pow(point.y, 2)) / screenInches;
    }
```

> - `refreshScaleView()` 为被调用的刷新比例尺数据的方法
> - `setScaleText()` 设置比例尺文字，这个不用说明
> - `setScaleWidth()` 这是比例尺的宽度，里面用到 `getPPIofDevice()`这方法来获取设备的PPI数据，用于显示实际宽度

---
题图：来源于 Google
