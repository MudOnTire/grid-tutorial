# 基本介绍

上一篇文章我们介绍了 [css3 flexbox](https://segmentfault.com/a/1190000017347626)，今天我们再来说说css3的另外一个强大的功能：Grid。
Grid做前端的同学应该都很熟悉了，翻译成中文为“栅格”，用过bootstrap、semantic ui、ant design的同学肯定都了解grid layout（删格布局），以往css框架中的grid布局一般是通过float和百分比的宽度实现的，这种实现有几种缺点：

1. html不够简洁；
2. 需要清除浮动以避免高度塌陷；
3. 列的个数是固定的，不能灵活定义。比如bootstrap是12列，semantic ui是16列，ant design 24列。

当然grid也可以用flex实现，但是并不会比用float简单多少，而且flex擅长的是一维空间的布局，而对grid这种二维空间并不擅长。现在css3从规范和标准层面实现了grid，编程体验大大提升！

# 兼容性

![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/7684cd03016e583cf115.png)

# 用法

Grid作为一个二维的栅格系统，由若干列（column）和行（row）构成。

## 1. Column

### (1) 设置column

CSS3中的Grid可以划分为任意个数的列（column），而且每个column的宽度可以任意设置！我们先来看一个简单的例子：

html:

```
<div id="content">
	<div>1</div>
	<div>2</div>
	<div>3</div>
	<div>4</div>
	<div>5</div>
	<div>6</div>
	<div>7</div>
	<div>8</div>
	<div>9</div>
</div>
```
css: 

```
body{
	color: #fff;
	text-align: center;
}
#content{
	display: grid;
	grid-template-columns: 33.3% 33.3% 33.3%;
	max-width: 960px;
	margin: 0 auto;
}
#content div{
	background: lightgrey;
	padding: 30px;
}
#content div:nth-child(even){
	background: skyblue;
}
```

效果：
![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/6739fba6169ead07b30f.png)

当我们设置了`display: grid`和`grid-template-columns: 33.3% 33.3% 33.3%`后`#content`就被划分成了三行三列的grid，此时`#content`被称为**grid container**，而`#content`的子元素称为**grid item**。

我们也可以任意改变column的个数和宽度，比如：

```
#content{
    grid-template-columns: 10% 20% 30% 40%;
}
```

效果：
![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/fc50039be5d6b067afed.png)

### (2) fr（fraction）

css3中引入了一个新的单位fr（fraction），中文意思为“分数”，用于替代百分比，因为百分比（小数）存在除不尽的情况，用分数表示可以避免多位小数的写法。比如三列等宽的grid可以表示为：

```
grid-template-columns: 1fr 1fr 1fr;
```

### (3) repeat

我们也可以用`repeat`方法来简化column或者row的写法，repeat方法接受两个参数，第一个参数表示重复的次数，第二个参数表示重复的内容。所以，三列等宽的grid我们还可以表示为：

```
grid-template-columns: repeat(3, 1fr);
```

当我们要定义的列数很多时，repeat就会变得非常有用，比如我们要定义一个10列等宽的grid，可以写成`repeat(10, 1fr)`，而不用将`1fr`重复书写10遍。

## 2. Row

### (1) 设置row

当我们设置column之后，row会因为元素的换行而自动产生，但是我们依然可以设置row的个数和高度。

css：
```
#content{
	display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: repeat(4, 60px);
	max-width: 960px;
}
```

效果：
![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/5e5153351081d04c194d.png)

可以看到，虽然第四行没有内容，但是row确实存在并占据了那部分空间。

### (2) minmax

上面的例子中我们给了row一个固定高度，这导致一个问题：如果某个grid item中的内容特别多，受制于固定的高度，部分内容将无法显示，如下图：

![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/fa9ed7fc7627b7c21936.png)

为解决这个问题，css提供了`minmax`函数，让我们可以设置row的最小高度和最大高度，最大高度取auto后便可以让row的高度自适应：

css：
```
grid-auto-rows: minmax(60px, auto);
// 或者
grid-template-rows: repeat(3, minmax(60px, auto));
```

效果：
![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/ebdbe61522c285592b40.png)

### (3) grid gap

如果我们想给行和列之间加上间隔，也有现成的方法：
css: 

```
grid-gap:{
  10px;
}
```

效果：
![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/3e0a853d733efa6d735c.png)

## 3. Grid Line

以上所有例子中，grid中的每个grid item都是按默认顺序排列的。如果我们想重新布局改变grid item的位置或大小呢？为此引入了grid lines的概念，所谓的grid lines就是将grid若干等分后的分割线，如下图中横向和纵向序号1～8的线即为grid lines：

<img src="http://lc-jOYHMCEn.cn-n1.lcfile.com/50ac15f114d690644698.png" alt="drawing" width="500"/>

通过定义grid item的起始和结束的grid line我们就可以实现对grid item位置和覆盖面积的控制。一个简单的例子：

html：
```
<div id="content">
    <div class="one">1</div>
</div>
```

css: 
```
 #content {
  display: grid;
  grid-template-columns: repeat(8, 100px);
  grid-template-rows: repeat(8, 100px);
  grid-gap: 10px;
}

.one {
  grid-column-start: 3;
  grid-column-end: 6;
  grid-row-start: 3;
  grid-row-end: 6;
}
```

效果：
![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/9bde847b7aeea43fb5e2.png)

通过设置`grid-column-start/end  grid-row-start/end` 相当于给grid item设置起始坐标和结束坐标，上面的css也可以简写为：

```
.one {
  grid-column: 3 / 6;
  grid-row: 3 / 6;
}
// 或者
.one {
  grid-area: 3 / 3 / 6 / 6;
}
```

如果grid item的起始grid line为默认，我们可以只设置它的跨度（span）:
```
.one{
  grid-column: span 3;
  grid-row: span 3;
}
```

## 4. Grid Area Template

除了通过grid lines进行布局，css3提供了一种更牛逼的布局方式：grid area template。与其用语言解释什么是grid area template，不如直接看代码：

html：

```
<div id="content">
    <header>Header</header>
    <main>Main</main>
    <section>Section</section>
    <aside>Aside</aside>
    <nav>Nav</nav>
    <footer>Footer</footer>
</div>
```

css:

```
body {
  color: #fff;
  text-align: center;
}

#content {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(100px, auto);
  max-width: 960px;
  margin: 0 auto;
  grid-gap: 10px;
  grid-template-areas: 
  "header header header header"
  "aside . main main"
  "nav . main main" 
  "section section section section"
  "section section section section"
  "footer footer footer footer";
}

#content>* {
  background: #3bbced;
  padding: 30px;
}

header { grid-area: header; }
main   { grid-area: main; }
section{ grid-area: section; }
aside  { grid-area: aside; }
nav    { grid-area: nav; }
footer { grid-area: footer; }
```

效果：
![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/de4849956b62f8e2027a.png)

看明白没有？重点在于grid container的 `grid-template-areas` 属性。我们给每个grid item设置一个grid area，然后在grid container中设置一个grid area模版（grid-template-areas），模版中每行字符串表示一个row，每个area名称表示一个column，完全将几何布局用文字模拟出来，空白的grid item用 `.` 表示。当然使用grid area要注意语法严谨，像 `"header main header main"` 这种写法css是无法解析的，用area名称模拟出的结构在二维空间上必须是一个整体，因为每个grid item也是无法分割的。 

**使用grid area template的有点在实现响应式布局时也是显而易见的，我们只需要针对不同的屏幕尺寸制定不同的grid area template就行了。**


## 5. Justify and Align

与flex类似，grid也可以设置justify和align来调整grid item横向和纵向对齐方式。同样也同时支持对grid container或单个grid item进行设置。

### 对grid container设置

html: 
```
<div id="content">
    <div class="one">1</div>
    <div>2</div>
    <div>3</div>
    <div>4</div>
    <div>5</div>
    <div>6</div>
</div>
```

css: 
```
#content {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(2, minmax(120px, auto));
  grid-gap: 10px;
  max-width: 960px;
  align-items: start;
  justify-items: end;
}
```

效果：
![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/9700664d3f29b8e1e776.png)

注意：flex里面用的是 `justify-content` 而grid里面是 `justify-items` ，flex里面的值是 `flex-start/flex-end` ，而grid里面是 `start/end` 。justify和align的默认值都是 `stretch` 。

### 对grid item设置

css：
```
.one{
  align-self: start;
  justify-self: end;
}
```

效果：
![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/0dc8e0a937736af59f5d.png)

# 实践

掌握了上述知识，我们就能用CSS3 Grid快速做出各种layout效果了，附上几个简单的codepen示例：

1. [12列grid布局](https://codepen.io/mudontire/pen/qLqmpE)
2. [花瓣式布局](https://codepen.io/mudontire/pen/xmRdWK)
3. [响应式布局](https://codepen.io/mudontire/pen/pqNwvB)