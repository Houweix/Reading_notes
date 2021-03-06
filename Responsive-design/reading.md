# 响应式Web设计：HTML5和CSS3实战

人民邮电出版社出版 2013年 1 月第 1 版

## 第一章 HTML5 、 CSS3 及响应式设计入门

响应式网页设计（RWD）：包含（弹性网格布局、弹性图片、媒体和媒体查询。

>针对任意设备对网页内容进行完美布局的一种显示机制

#### 视口和屏幕尺寸：
**视口**：是指浏览器窗口内的内容区域，不包含工具栏、标签栏等，也就是网页的实际显示区域。

**屏幕尺寸**：指的是设备的物理显示区域。

## 第二章 媒体查询：支持不同的视口

没有**CSS3**的媒体查询模块，就不能针对设备特性（如视口宽度）设置特定的 CSS样式。

[MDN媒体查询](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Media_queries)

可以在`link标签`中使用媒体查询，也可以在`CSS样式表`中使用。

### 媒体查询可以检测哪些特性？

* width ：视口宽度。
* height ：视口高度。
* device-width ：渲染表面的宽度（对我们来说，就是设备屏幕的宽度）。
* device-height ：渲染表面的高度（对我们来说，就是设备屏幕的高度）。
* orientation ：检查设备处于横向还是纵向。
* aspect-ratio ：基于视口宽度和高度的宽高比。一个 16∶9 比例的显示屏可以这样定义 aspect-ratio: 16/9 。
* device-aspect-ratio ：和 aspect-ratio 类似，基于设备渲染平面宽度和高度的宽高比。
* color ：每种颜色的位数。例如 min-color: 16 会检测设备是否拥有 16位颜色。
* color-index ：设备的颜色索引表中的颜色数。值必须是非负整数。
* monochrome ：检测单色帧缓冲区中每像素所使用的位数。值必须是非负整数，如
* monochrome: 2 。
* resolution ：用来检测屏幕或打印机的分辨率，如 min-resolution: 300dpi 。还可以接受每厘米像素点数的度量值，如 min-resolution: 118dpcm 。
* scan ：电视机的扫描方式，值可设为 progressive （逐行扫描）或 interlace （隔行扫描）。如 720p HD电视（720p的 p即表明是逐行扫描）匹配 scan: progressive ，而 1080i HD 电视（1080i中的 i表明是隔行扫描）匹配 scan: interlace 。
* grid ：用来检测输出设备是网格设备还是位图设备。

### 2.4 阻止移动浏览器自动调整页面大小

iOS和安卓的浏览器都是基于webkit核心，都支持使用viewport meta元素覆盖默认的画布缩放设置。只需要在head标签中插入一个meta标签。

下面是一个缩放2倍的实例：
```
<meta name="viewport" content="initial-scale=2.0,width=device-width" />
```

### 2.6 响应式设计中内容始终优先

窄视口设备的用户应该先看到主内容，而后再看到侧边栏。


## 第三章 拥抱流式布局

>使用百分比布局创建流动的弹性界面，同时使用媒体查询来限制元素的变动范围。将这两者组合到一起构成了响应式设计的核心，基于此可以创造出真正完美的设计。

### 3.3 将网页从固定布局修改为百分比布局

公式：
    **目标元素的宽度 ÷ 上下文元素的宽度 = 百分比宽度**

这个公式也适用于将文字的像素单位转换为相对单位，最终转换px为em。

### 3.5 弹性图片

在现代浏览器（包括 IE 7+）中要实现图片随着流动布局相应缩放非常简单。只需在 CSS中作如下声明：
```
img {
    max-width: 100%;
}
```

但这个方法有几个问题：

1. 要提前准备一张足够大的图片，以备大视口使用。但这也就引出了第二个。
2. 无论视口多大，什么设备，都得下载超大图片。可能对某些设备来说，图片大小只要原始图片的 25%就好了。另外，在某些情况下，你还不得不因此而考虑带宽限制。

如果需要为图片自指定特殊规则：
```
//通过选择器
.sideBlock img {
    max-width: 45%;
}
```

### 3.5.3 给弹性图片设置阈值
现在图片可以随着视口的伸缩而缩放了。但是如果将视口拉大，直到图片拉伸至超出其原始尺寸那就麻烦了。

当我们使用公公式计算得出`百分比宽度`后如果需要给图片设置阈值，则可以设置max-width属性。或者给父元素设置（比如#wrapper设置）max-width属性。

### 为不同的屏幕尺寸提供不同的图片

> Matt Wilcox 的“自适应图片”：根据标签中已经设定的全尺寸图片自动创建各种尺寸的图片。这种解决方案允许基于一组屏幕尺寸断点，根据用户需要为其提供不同的图片。

### CSS网格系统

>在平面设计中，网格是一种由一系列用于组织内容的相交的直线（垂直的、水平的）组成的结构（通常是二维的）。它广泛应用于打印设计中的设计布局和内容结构。在网页设计中，它是一种用于快速创建一致的布局和有效地使用 HTML 和 CSS 的方法。

Bootstap网格系统

## 响应式设计中的HTML5

`polyfill腻子脚本`
>腻子脚本（polyfill）这个词由 Remy Sharp提出，意指使用腻子来补平老版本浏览器的缺陷。因此，腻子脚本具体指的是一段能给老版本浏览器带来新特性的 JavaScript代码。


### 渐进增强与优雅降级

>**优雅降级**指的是为现代浏览器制作网站，然后保证为某些老版本浏览器提供基本可用的体验。新特性在老版本浏览器中会降级，且一般会有一个分界点，声明不支持那些老掉牙的浏览器。有些时候用户也仅会被警告他们所使用的浏览器有问题，建议其更换（如“您的浏览器老得让人笑话——建议下载最新版浏览器！”）。

>**渐进增强**与优雅降级恰好相反。渐进增强以恪守 Web标准的标签为基础，意味着它在所有浏览器中均可用。然后通过 CSS 样式和必要的 JavaScript 来为更先进的浏览器提供渐进式的增强体验。

