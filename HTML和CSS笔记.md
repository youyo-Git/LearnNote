---
title: HTML和CSS笔记
tags:
  - 笔记
  - 总结
categories: 学习
toc: true
author: youyo
abbrlink: 20244
---

### 《前言》

#### 1. 互联网行业

圆形图

1. 产品经理 ：提需求的
2. UI设计师 ：把产品经理的线框图 转化为设计稿
3. 前端开发工程师 实现为一个真正的程序 跟浏览器打交道的
4. 后端开发工程师 操作数据的 跟服务器打交道的
5. 测试 工程师 找bug的
6. 运维工程师 项目上线的维护

​	 ios开发工程师
​ 	 安卓开发工程师

#### 2. 我们的工作范畴
pc端：
	网站：电商网站 企业站
	后台管理系统：互联网办公的
移动端：
	app:ios开发工程师和安卓开发工程师
	小程序：
	h5：web app

#### 3. 世界上五大浏览器
1. chrome(谷歌)
2. firefox(火狐)
3. IE Safari(苹果)
4. opera(欧鹏)

##### 代码编辑器

1. vscode
2. Hbuilder

##### html css
html:负责网页结构的
css:负责网页样式的
html:由标签组成
单标签：没有内容
双标签：开始标签和结束标签 可以放内容的
所有写在元素开始标签之中的都是该标签的属性

```txt
<!DOCTYPE html>：用来声明文档类型
<html></html> 代表了整个网页
<head></head>用来写网页的一些配置信息
<meta charset="UTF-8"> 声明文档的编码格式
<title></title> 负责网页标题
<body></body>负责网页内容
```

格式：标签换行， 开始标签和结束标签要对齐（一条竖线）
子父级之间相差一个tab键

### 网页布局
#### 1. 画块
div

#### 2. 给元素/标签加样式
1. 内联/行内样式：写在元素开始标签里面的方式
2. 内部样式：写在head里的style标签里 需要大括号包裹
   class属性：可以重命名
   class命名规范：不能用中文，不能用特殊符号（除了_-）不能用纯数字 不能以数字打
   头，不能用大写字母，不能有空格 见名知意
   id：不能重名
3. 外部样式
   `<link rel="stylesheet" href="1.css"/>`
   href后面跟的是路径
   绝对路径：不需要参照物 http://www.baidu.com
   相对路径：有参照物的,参照的是当前文件
4. 样式的优先级
   !important>内联样式>内部样式(id>class>标签名)(后>前)
   10000 	1000 		  100  10    1

   内部样式=外部样式

#### 3. 改变元素位置
1. margin(外边距)：<font color='orange'>值可以给负数</font>
   问题：
（1） margin塌陷：子父级之间，顶部想隔开距离，给子集加margin-top会发生
塌陷，也就是这个margin会加在父级身上，导致父级整体往下移（浮动元素除外）
（2）margin合并：在垂直方向上，同级，临边margin会合并为一个，取较大值
2. padding(内边距)
   问题：
   会自动的增宽或者增高，所以我们要主动的减去对应的宽高
3. position(定位)  <font color='cornflowerblue'>更改层叠顺序，`z-index`只能定位元素使用</font>
（1） absolute(绝对定位)：想改变谁的位置，就给谁加绝对定位，需要给top left right
   bottom，绝对定位的元素默认参照的是已定位的**父级**元素，就近
（2） realtive（相对定位）:想以谁当作参照，就给谁加相对定位

#### 4. 元素的默认宽高
1. 默认宽度：父级的百分百
   浮动元素默认宽度由子集决定
   
2. 默认高度由子集高度+padding决定

   <font color='orange'>直接设置宽度应该不要大于1200px</font>

#### 5. 元素
1. 块元素：div ul li h1-h6(标题标签) p(段落标签)
   table(表格) rowspan(占几行) colspan(占几列)
   （1）独占一行
   （2）有宽高
2. 内联/行内元素：span a(超链接) img(图片标签)   <font color='cornflowerblue'>内联元素有margin和padding，但只有左右生效</font>
   input的属性 placeholder(占位符) value(值) type(类型) name(给服务器用的名字)
   button
   （1）不独占一行的，当父级容不下的时候会换行
   （2）内联元素没有宽高
   元素的嵌套：块元素可以包含块和内联元素，内联元素里只能放内联元素

#### 5. 浮动
**可以让元素在一行上显示**
希望哪几个元素在一行上显示，那么就给哪几个元素加浮动
**float:left/right**
问题：脱离文档流不占位置
解决：在浮动的元素元素后面添加一个元素，样式为clear：both
**clear:both** 需要放在所有浮动元素之后，并且是同级关系。页面中有几处浮动，那么
就需要清除几次

#### 6. 划块原则
从大到小，依次划分
**同级元素不能既在一行上，又在一列上**
所以：清除浮动的元素一定是作为该层级的最后一个元素

<font color='orange'>对于不规则的样式，先想成规则的再仔细观察，合理使用负数值。</font>

#### 7.css选择器
1. #id
2. .class
3. tag(标签)
同时找多个，中间用逗号，如  `.class1, .class2, .class3{...}`
找子级中间用空格，如找nav下子级class1 `.nav .class1`
找直接子级中间用>隔开，如找nav下直接子集class1 `.nav>.class1`

<font color='cornflowerblue'>+，:first-child</font>

#### 8. display
1. block：转为块元素
2. inline：转为内联元素
3. inline-block：转为内联块，相当于不独占一行的块级元素
4. none：无

#### 9. 文字样式
1. `text-align: center` 文本对齐方式，让文本居包含文本的这个块的中间
    某元素加了文字样式，那么对它里面的文字生效  <font color='cornflowerblue'>对内联元素也生效</font>

2. `color: #ff6637`  文字颜色

3. `font-family:` 宋体  文字字体

4. `font-size: 16px` 文字大小

5. `font-weight: bold` 文字粗细

6. `line-height: 100px` 垂直居中：只需要把行高设置为跟外层div高度一致即可
    任何文字样式都是可继承的

7. `text-indent: 2em` 首行缩进2个文字大小

  <font color='cornflowerblue'>文字有自带的行高</font>

  

#### 10. 居中
**任何情况下不要拿显示器边缘当参照**

1. auto：自动计算剩余空间
   `margin: auto` 给**块**做居中，前提必须得有宽度，<font color='orange'>用了定位无法用margin：auto</font>
2. <font color='cornflowerblue'>定位百分比+margin负数</font>
3. 定位百分比+transform:translate(-50%,-50%)



#### 11. 伪类
元素：hover{鼠标滑过后的样式} 如下*鼠标滑过li元素 则改变背景和字体颜色*
```css
.nav li:hover{background: #5696ff;color: #fefeff;}
```
<font color='orange'>:hover修饰li（相当于 li:hover是一个整体），选择器用法不变</font>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style>
        .par{width: 300px; height: 300px; background: red;}
        .child{width: 100px; height: 100px; background: pink;}
        .par>.child>.child{width: 40px; height: 40px;background: brown;}
        .btn{width: 100px; height: 50px; background: yellow;}
        .par:hover+.btn{width: 100px; height: 100px; background: blue;}
        /* .par:hover .child{border: 5px solid black;} */   /*所有子集变样式*/
        .par:hover>.child{border: 5px solid black}          /*直接子集变样式*/
        #cl:hover{background: green;}
        
    </style>
</head>
<body>
    <div class="par">
        <div class="child" id="cl">
            <div class="child"></div>
        </div>
    </div>
    <div class="btn"></div>
</body>
</html>
```



#### 12. 文字样式

font-weight（字体加粗） color（文字颜色） font-size（字体大小） text-align（水平对齐
方式） line-height（垂直对齐方式） font-family（字体类型）

#### 13. a标签
`<a href="" target="">`

1. `target="_blank"` 代表在新标签页打开，默认为`_self`,在当前窗口刷新

2. href="路径" 可以是绝对路径也可以是相对路径

   <font color='orange'>去掉下划线：`text-decoration: none`</font>
   
   <font color='orange'>underline</font>
   
   <font color='orange'>overline</font>
   
   <font color='orange'>line-through: 中划线</font>
   
   

#### 14. 引入图片的两种方式
1. img标签直接引入<font color='cornflowerblue'>图片默认底对齐方式，以底部为基线；`vertical-align: middle` 中间线对齐，仅针对内联元素</font>
2. 通过背景图方式引入
   **什么时候用背景图方式引入:**
   1. 如果图片可能超出显示器尺寸的时候，选择用背景图
   2. 如果图片里面还有内容，需要继续编辑，选择用背景图
   3. 其他情况推荐使用img标签

#### 15. 描边、圆角、半透明
1. border: chocolate(颜色) 2px(粗细) solid/dashed(直线/虚线)<font color='cornflowerblue'>描边默认在外边，`box-sizing: border-box`将描边加在里边</font>

2. border-radius 1px(圆角) <font color='cornflowerblue'>最大只有宽高的一半，圆或者椭圆</font>

3. opacity:0.5 半透明 会导致元素**里面的内容也变成半透明** <font color='cornflowerblue'>值在0~1之间</font>

4. background-color:rgba(51,51,51,0.5) **不会**导致里面内容也变成半透明 rgba中最后一个参数的范围为0~1

   <font color='cornflowerblue'>搜索框去蓝边，`outline: none`</font>

#### 16. 小demo 实现二级下拉菜单
鼠标滑过一个元素改另一个元素的样式
鼠标要滑过的元素:hover 要更改样式的元素{}
注意：要更改的元素必须是鼠标滑过的元素**子集**

#### 17. 投影
box-shodow：水平偏移距离 垂直偏移距离 模糊程度 投影大小 投影颜色  <font color='orange'>投影不占位置
box-shadow不要上边框，可以用上面元素来覆盖该元素（已定位，用z-index层叠）的上边框
</font>

#### 18. 清除浮动的方式总结（四种）
1. clear:both 不做详细描述
2. 使用css的overflow属性，给浮动元素的容器添加overflow:hidden;或overflow:auto;可以清除浮动   <font color='cornflowerblue'>`overflow:hidden`子集超过父级部分隐藏，`overflow: auto`自动不超出父级，并给个滚动条能够完全展示子集内容，`overflow-x: scroll`主动增加（横向）滚动条，</font>
3. 给浮动的元素的容器添加浮动
4. 使用CSS的:after伪元素
    注意这不是伪类，而是**伪元素**，代表一个元素之后最近的元素。如下
```html
<!DOCTYPE html>
<html>
<head>
<title></title>
<meta charset="utf‐8">
<style type="text/css">
    .yellow{width: 800px;background: yellow}
    .red{width: 200px;height: 200px;background: red;float: left;}
    .blue{width: 200px;height: 200px;background: blue;float: left;}
    .yellow:after{content: "";
        display: block; 
        height: 0;
        clear: both;
    }
</style>
</head>
<body>
    <div class="yellow">
        <div class="red"></div>
        <div class="blue"></div>
    </div>
</body>
</html>
```

#### 19.  不会发生margin塌陷的几种情况
1. 为父盒子设置border，为外层添加border后父子盒子就不是真正意义上的贴合 （可以设置成透明：border：1px solid transparent）。<font color='cornflowerblue'>准确来说就是加border-top,加左右是不会影响的，并且可以让边框透明，这样就看不出来了，transparent；</font>
   
2. 为父盒子添加overflow：hidden；<font color='cornflowerblue'>不推荐</font>

3. 为父盒子设定padding值；   <font color='cornflowerblue'>直接设置padding解决塌陷最好</font>

4. 为父盒子添加position：fixed；<font color='cornflowerblue'>不推荐</font>

5. 利用伪元素给子元素的前面添加一个空元素  （暂时不看，笔记有误）
   ```html
   .son:before{ content:"";
    overflow:hidden;} 
   ```

6. 子元素浮动也不会塌陷；<font color='cornflowerblue'>给父级加浮动也可以实现但副作用很大</font>

#### 20. 百分比问题
百分比数参照物
父元素宽度：padding，margin，width
父元素高度：height 如父元素为body不生效
自身：border-radius
已定位的父级:position:



#### 21. css中的单位
px
em: 1em等于一个文字所占大小
rem: 1rem等于**根元素**(html)一个文字所占大小(重要单位，响应式布局)

<font color='cornflowerblue'>百分比：相对于父级元素</font>，<font color='orange'>body的height不能直接获取，需要能直接获取height的父级</font>

<font color='cornflowerblue'>vh: 把显示器的高100等分，1vh就是1份那么大 </font>

<font color='cornflowerblue'>vw：把显示器的宽100等分</font>



#### 22. 表单

<font color='cornflowerblue'>内联块，input: value，type， placehoder</font>

<font color='cornflowerblue'>radio: 单选框，checkbox:复选框，select option：下拉选择框</font><font color='orange'>是一系列需要用相同的name</font>

<font color='cornflowerblue'>文字强制不换行：white-space:nowrap</font>, 比如人名不能换行。




---

### HTML  CSS3

1. video

   ```html
   <video width="320" height="240" controls>
   	<source src="movie.mp4" type="video/mp4">
   </video>
   ```
   属性：controls视频播放控件（进度、音量、全屏、播放）
   	       autoplay 自动播放
   	       loop 循环播放
   
    ```html
    <audio controls>
   	<source src="music.mp3" type="video/mp3">
    </video>
    ```
2. html语义化

   	header footer nav aside   <font color='cornflowerblue'>语义元素</font>

css3 
   	tranform(转换) 2d  
   		rotate:旋转  单位deg（度）<font color='cornflowerblue'>标签都能旋转</font>
		   translate:偏移 针对你**当前位置**的基础上
		   skew:倾斜
		   scale:缩放 物理缩放 *不占位置 不影响结构* <font color='cornflowerblue'>同时也会影响里面的结构</font>
	transform 3d
		   rotateX
		   rotateY
	transition：过渡 让样式的变化有一个过程(动画)
		   animation:动画
	响应式布局的
	@media screen
	

#### 22.响应式布局
   1.百分比 最简单最常用
    2.@media screen 最强大最复杂的
    3.rem 最适用于移动端
    4.flex(弹性)布局 ：粗略布局，不适用于精确计算
    给容器加display:flex ,那么该容器内的元素就遵从flex布局
    ```html
    flex-direction: 更改元素的排列方向
    row(默认值):里面的子集在一行排列
    row-reverse:在一行排列，起点在右端
    column:垂直排列
    column-reverse:从下往上
    flex-wrap:定义元素的换行方式
    nowrap:不换行，如果容不下，自动等比缩小
    wrap:换行
    wrap-reverse :反向排列
    justify-content:水平方向上的对齐方式
    flex-start:默认从左往右
    flex-end 从右往左
    center:居中对齐
    space-between:等间距排列（两端定边，中间等间距）
    space-around:等间距排列
    align-items:垂直方向上的对齐方式
    flex-start：从上面排布
    flex-end:从下面排布
    center:居中
    stretch:如果没有高度将会充满容器
    align-content:多行时的对齐方式
    flex-start:顶上排列
    flex-end:顶下排列
    space-between:等间距排列（两端定边，中间等间距）
    space-around:等间距排列
    ```



### PS的一些使用方法：

1. 取消自动选择图层，主动选图层按ctrl+鼠标点击
2. 隐藏参考线：ctrl+;
3. 显示刻度尺：ctrl+r
4. 矩形选框：ctrl+d  取消选框，按住空格可以移动选框



### 碰到的一些名词

SEO：(Search Engine Optimization), 译为搜索引擎优化也叫网站优化技术，利用搜索引擎的规则提高提高网站上有关搜索的自然排名。

### 面试题系列
画三角: 对角线切割 实心边框
```html
<style>
    .div1 {
        width: 0;
        height: 0;
        border: 100px solid transparent;
        border-bottom-color: red;
    }
</style>

<body>
    <div class="div1"></div>
</body>
```

