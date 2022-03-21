---
layout: post
title:  "React Native 屏幕适配"
author_github: cammytang
tags: react-native css 
category: frontend
---
# React Native 屏幕适配

## 问题
设计稿使用px, React Native 无单位，用的设备逻辑单位pt/dp。如何适配不同设备屏幕尺寸以及不同分辨率？


## 一些基础概念
**pt vs dp**  
+ 相同点：  
都是为了适配不同屏幕不同尺寸的移动设备而设计出来的一种单位，用于表示UI布局尺寸与设备屏幕密度无关。所以又叫密度无关像素或者密度独立像素。（Chrome debug模式下的设备模拟器，看到的就是这个尺寸）
+ 不同点：  
pt是iOS设备的布局尺寸单位，苹果公司给的定义是：1pt = 1/163 inch，dp是android设备的布局尺寸单位，Wikipedia给的定义对于android设备：1dp = 1/160 inch；
像素(pixels)和二者的转换关系不同：iOS pt = pixels * ( ppi/163)、Android dp = pixels * (ppi/160)

**屏幕尺寸**：屏幕对角线的长度，比如iPhoneX是5.8英寸。（英寸：inch，1英寸=2.54厘米）
dpi/ppi：屏幕密度，每英寸的点数/每英寸的像素值。（比如 320ppi表示每平方英寸有320 * 320 个 px）
ppi=x2+y2屏幕尺寸

**分辨率**：屏幕上物理像素的总数，横向和纵向的像素值 比如 iPhoneX的分辨率是 1125px* 2436px

**如何理解React Native是无单位的?**  
首先是因为 React Native 是跨平台的，iOS的逻辑像素单位是 pt，Android的逻辑像素单位是 dp，没办法兼顾，就直接去掉了，运行在哪个平台，就用哪个平台的单位。
RN 提供的就是 逻辑像素单位，是和像素密度无关的，RN本身会根据像素密度做适配，渲染对应的物理像素。


## 解决方案
### 场景：布局
整体布局一般采用Flex 弹性布局。特别的全屏布局或百分比布局，利用 屏幕宽度/高度 计算而来，
```
const {deviceWidth, deviceHeight} = Dimensions.get('window');
width: deviceWidth * 0.7; 
```       

比如整个页面的布局是header，footer 固定height，中间自适应. 中间区域可以利用高度计算而来。

![layout]({{ site.cdn_prefix }}assets/images/assets/2022/03/21/mobile_design.png)

比如这个item row的布局，目前用的是flex, 1:3:1:1:1

![layout]({{ site.cdn_prefix }}assets/images/assets/2022/03/21/item.png)


### 场景：盒模型 
局部盒模型元素，进行宽度适配，保证页面元素大小按照设计稿尺寸进行等比例缩放。（盒模型中的width,height,padding,margin...)

举例：  
设计稿：整体尺寸 1920px * 1080px. 设计了一个元素 640px * 540px(蓝色区域)
![design]({{ site.cdn_prefix }}assets/images/assets/2022/03/21/demo_design.png)

最佳实践
```javascript
//获取设备dp
const designTotalWidth = 1920; // 设计稿尺寸 1920px*1080px
const {deviceWidth, deviceHeight} = Dimensions.get('window');
//设计稿上一个 640px*540px 的区域。CSS
{
    width:   (640/designTotalWidth) * deviceWidth
    height:  (540/designTotalWidth) * deviceWidth
}
```
来看实际情况  

|设备|React Native CSS|Device UI|
|:---------------------------------------------------------------------------|:-----------------------------|:----------------------------------------------------------------------------------------|
|设备(1080P) 960dp * 540dp，分辨率1920 *1080， density 320dpi, xhdpi, 像素密度 2|根据公式:width: 320,height: 270|![device result]({{ site.cdn_prefix }}assets/images/assets/2022/03/21/device_radio_2.png)|
|设备(4K) 960dp * 540dp，分辨率 3840 *2160,   density 640dpi, xhdpi, 像素密度 4|根据公式:width: 320,height: 270|![device result]({{ site.cdn_prefix }}assets/images/assets/2022/03/21/device_radio_4.png)|


*结论*：**蓝色盒子始终是整个屏幕的⅙ ，宽度是整个屏幕的 ⅓，高度是½.  实现了屏幕适配。这个解决方案会最大限度的保留设计图的大小与布局设计效果。**

比如红色区域的宽高，黄色区域的宽度  
![layout]({{ site.cdn_prefix }}assets/images/assets/2022/03/21/tv_design.png)

*进阶抽离*
将 px2dp 等比例转化方法提取出来，按需使用。
这里也有一种方案是整体做了处理，缺点是内部一些特别的元素无法区别开


### 场景：图片
准备多个分辨率尺寸的图片，通过设备的像素密度比，拿到最适合的图片进行显示
```javascript
const image = getImage({
  width: PixelRatio.getPixelSizeForLayoutSize(200),//如果当前的设备 pixelratio 为2，则返回 width:400, height:200
  height: PixelRatio.getPixelSizeForLayoutSize(100),// 等同于 100 * PixelRatio.get()
});
<Image source={image} style={{width: 200, height: 100}} />
```
参考官网：https://reactnative.cn/docs/pixelratio

### 场景：字体

1. 需要根据设备字体缩放比例
  
   利用 getFontScale(), Android 返回设备设置的字体缩放比例，iOS 返回默认的像素密度
fontSize: 12 * getFontScale();
2. 根据设备屏幕大小自适应
  
   有很多的换算方法，总的来说，要么根据像素密度比，要么根据屏幕缩放比，要么根据屏幕尺寸分区
  https://github.com/CodeRabbitYu/ShiTu/blob/master/app/utils/FontSize.js
3. 需要自适应父容器的高宽
   
   ```
   <View style={height:100,width:200}>
         <Text style={{fontSize: 28, color: 'red', textAlign: 'center'}} adjustsFontSizeToFit={true}>
             Example of adjustsFontSizeToFit True in React Native
          </Text>
   </View>
   ```
![layout]({{ site.cdn_prefix }}assets/images/assets/2022/03/21/font.png)

*进阶抽离*  
可以写一个custom Text，在内部进行字体大小转换，返回原生 Text。

*思考*：**对于字体的问题，从用户体验来说用户心愿看到的字体是相对大小的，就像看报纸，多大张的报纸，字体都应该是一样大，有一个最适宜浏览的字体大小，而自适应突破了这种设计哲学。**


### 场景：边框或线条
边框和线条一般要求所有屏幕保持相同的px 显示，比如需要看到一条最细的边框线。
```javascript
borderWidth: 1/PixelRatio.get()
```
![layout]({{ site.cdn_prefix }}assets/images/assets/2022/03/21/line_border.png)

**像素精度**  
比如设计稿上有个尺寸 width: 8.6 我们应该如何使用？
折算到匹配整数像素的 dp 尺寸
```javascript
width: PixelRatio.roundToNearestPixel(8.4) 
```

###不同设备屏幕尺寸
在进行 Web UI开发时，我们一般会针对不同的区间做响应式设计，不同的区间在设计上也会有一些区别。
USGA实际上设计了两个区间 phone 和 TV，但我们并没有以逻辑像素来判断，用的是上层给的 mode

+ 主流的Phone 屏幕尺寸及分辨率
https://uiiiuiii.com/screen/index.htm

+ 主流的TV屏幕尺寸  
1080*1920 -xhdpi (dpi=1.5)  
720*1280 - mdpi   (dpi=1.0)  
3840*2160 - xhdpi (dpi=4.0)  

我们目前的在 TV 上的方案：
```javascript
width: 40 / PixelRatio.get()
```

完成的效果是，不同分辨率和不同屏幕大小的设备下，宽度始终是 40px
效果会是这样的：在低密度上看起来比较大，高密度上看起来比较小

回答之前Cool的问题，为什么我们只针对TV做，mobile什么也不做是好的？
看mobile的设计稿，给的是 375px * 646px, 我们测试的mobile 设备iphone 11 1792px * 828px, 像素密度 326/160
再看TV的设计稿，给的是 1920px * 1080px，我们测试的TV设备 (Android TV 1080P) 960dp * 540dp，像素密度 2，对应屏幕分辨率 1920px * 1080px

## 参考
+ React Native设计稿匹配：https://iiong.com/react-native-design-match-matching/
+ 聊聊React Native屏幕适配那些事儿：https://segmentfault.com/a/1190000039805723
+ 屏幕尺寸、分辨率、像素密度三者关系：http://www.noobyard.com/article/p-axrxhjor-rt.html
+ iOS和android布局开发单位对比(pt VS dp):https://juejin.cn/post/6844903536912760840
+ 移动端样式开发小结: https://juejin.cn/post/6983852401543348231
+ 美团：​​https://tech.meituan.com/2019/09/26/open-source-react-native-component-library-beeshell.html
+ ui 组件库: https://blog.csdn.net/weixin_50001396/article/details/116423588
+ 显示一条1px的线: https://github.com/susanwater/react-native-1/blob/master/pixel-ratio.md
+ React Native 分辨率适配 【进阶，提取转化】
  - https://www.jianshu.com/p/7836523b4d20
  - https://blog.actorsfit.com/a?ID=00600-084279b6-31a1-4475-aa41-7835987ac415
+ 一文解决React-Native的屏幕适配  【封装 CustomStyleSheet 代替原生 StyleSheet】https://juejin.cn/post/6889339410546950152

