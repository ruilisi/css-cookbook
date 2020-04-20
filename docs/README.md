# 快速开始

🚀 本文档主要记录在写前端CSS时遇到的问题及解决方法，持续更新中。



## 前端技术栈与工具（学习路线）

工具：**zsh + tmux + vim (SpaceVim)**

技术栈：**React + [Ant-Design](https://ant.design/index-cn) + CSS3 + Javascript + ES6 + Next.js + Redux**



## CSS命名规范

1. 没有任何理由的情况下就用worda-wordb的命名方法。
2. 由具体单词组成的composable css用A-B命名，譬如float: left，就写成F-L。
3. 比较复杂的常用combo，一般我们用骆驼命名法，譬如alignCenter。



## React事件命令

React事件的命名采用小驼峰式（camelCase），不是纯小写。

例如：

```react
<button onClick={activateLasers}>
	Activate Lasers
</button>
```



## div高度宽度的设置

### div高度设置适配页面

*Question*：页面分为两块，上和下，怎么使两块区域占满整个屏幕。

*Solution*：
设置最上层div高度为`height: 100vh`，上的高度为具体的高度，譬如60px，下的高度为`calc(100vh - 60px)`。

### div宽度设置适配页面

*Question*：两个div占一行，第一个div的宽度是60px，如何设置第二个div的宽度使两者占满整个屏幕。

*Solution1* : 

```js
	<div className="container">
		<div className="div1">1</div>
		<div className="div2">2</div>
	</div>
```

```jsx
	.container {
		width: 100%;
		height: 100vh;
	}
	.div1 {
		float: left;
		width: 80px;
	}
	.div2 {
		margin-left: 80px;
	}
```

*Solution2* :

使用[Ant-Design](https://ant.design/index-cn) 的[Grid栅格系统](https://ant.design/components/grid-cn/) 。

```js
<Row>
	<Col span={8} />
	<Col span={16} />
</Row>
```



## 多个div在同一行

*Question*：如何使多个div标签在同一行？

*Solution1*：
设置每个div的style样式：`display: inline-block`。

使用`inline-block`同时也有问题，比如，使得`Icon`图标和一个div在同一行，但是两个行高不一样。原因是默认情况下使用`inline-block`属性时，并列显示的元素的垂直对齐方式是底部对齐，为了使垂直对齐方式改为顶部对齐，就需要在div元素的样式中加入`vertical-align`属性。

*Solution2*：
使用`display: flex`属性。

引申：
[div等块级标签横向排列的方法总结](https://blog.csdn.net/zmhawk/article/details/73293366)

[Flex布局教程-阮一峰](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html?utm_source=tuicool)

### 两个div在同一行，分别在最左边和最右边

*Solution1*:

```js
<div>
  <div style={{ display: 'inline-block'}}>aaa</div>
  <div style={{ float: 'right' }}>bbb</div>
</div>
```

*Solution2*:

```js
<div style={{ display: 'flex', justifyContent: 'flex-end'}}>
	<div style={{ display: 'flex', width: '100%'}}>aaa</div>
	<div style={{ display: 'flex'}}>bbb</div>
</div>
```



## div固定在页面底部

*Solution*：

将父div设置为`position: relative`，然后将子div样式设置为：

```CSS
	position: absolute;
	bottom: 0;
```



## div内文字超过宽度自动换行

*Solution*：

div设置宽度后style样式加上`word-break: break-all;`或`word-wrap: break-word;`

`word-break: break-all`：例如div宽度为200px，它的内容超过200px会自动换行，如果该行末尾有个很长的单词（congratulation），它会把单词截断，变成该行末端为congra，下一行为tulation。

`word-wrap: break-word`：与上面相同，但区别是它不会截断单词，它会把congratulation整个单词看成一个整体，自动把整个单词放到下一行，不会把单词截断。



## CSS hover效果总结

[CSS-鼠标移入移除样式改变，并有过渡效果](https://www.hangge.com/blog/cache/detail_982.html)



## Antd修改组件样式（antd-v3）

### 修改Modal背景颜色

```js
<Modal className="modal-dialog" />
```

由于jsx不支持嵌套，所以在main.scss中添加modal-dialog样式，

```css

.modal-dialog {
 .ant-modal-body {
  	background: #ffffff;
  }
}
```

### 修改Menu的border-right样式

![menu-example](/_media/antd-menu-example.png)
因为要修改的地方在`ant-menu-vertical`，所以不能直接在`Menu`标签里面添加`className`，而是要在它上一级div里面添加`layout-border`。

**!!!一定要注意逗号和空格**

```css
// main.scss

 .layout-border {
    .ant-menu-inline, .ant-menu-vertical, .ant-menu-vertical-left {
      border-right: none;
    }
  }
```

### Menu组件

`Menu`标签下不能加别的标签，否则会有warning，必须放在`<Menu.Item>...</Menu.Item>`里面。



## Antd自定义Icon图标

antd从4.0开始，不再内置Icon组件，使用独立的包`@ant-design/icons`。由于antd自带的图标不一定符合开发需求，所以有时会选择自定义图标。

本文使用的方法最简单，使用[iconfont.cn](iconfont.cn)，iconfont.cn是阿里的矢量图标库，里面有大量图标可供选择。将iconfont.cn上生成的js地址复制到代码中，即可实现渲染图标。

![iconfont](/_media/iconfont.png)

```js
	import { createFromIconfontCN } from '@ant-design/icons'
	
	const MyIcon = createFromIconfontCN({
		scriptUrl: '//at.alicdn.com/t/font_1675145_7w7tb9mha85.js'
	})
	
	<MyIcon type="icon-bianji"/> // type的值为图片下方的代码 
```

![iconfont_1](/_media/iconfont_1.png)



## 给项目安装webpack-bundle-analyzer

webpack-bundle-analyzer官方文档：https://www.npmjs.com/package/webpack-bundle-analyzer

通过使用webpack-bundle-analyzer可以看到项目各模块的大小，可以按需优化。

![webpack_1](/_media/webpack_1.png)

安装步骤：

1. 安装依赖包

   ```bash
   $ yarn add @next/bundle-analyzer
   or
   $ npm install @next/bundle-analyzer
   
   $ npm i --save-dev cross-env
   
   $ npm i --save-dev faker
   ```

2. 编辑`package.json`中`scrtip` 加上 `"analyze": "cross-env ANALYZE=true yarn build"`

   ![webapck_2](/_media/webpack_2.png)

3. 编辑`next.config.js`，注意`isDev`

   ![webpacl_3](/_media/webpack_3.png)

4. 运行`yarn analyze`就可以生成两个页面文件，`client.html` 和`server.html`。



## Antd中Input与Button在一行显示

*Question*：

要使`Input`输入框和`Button`按钮在一行。

*Solution1*：

```js
<Row>
  <Col span={16}>
  	<Input value={value} onChange={onChange} />
  </Col>
  <Col span={8}>
  <Button onClick={onClick}>{text}</Button>
  </Col>
</Row>
```

这种方法使用`Row`，`Col`组件，虽然代码精简，但同时也存在问题，`Input` 和`Button`的宽度无法占满整一行。

*Solution2*：

```js
 <Input.Search
          enterButton={text}
          value={value}
          onChange={onChange}
          onSearch={onClick}
 />
```

这种方法下，`Input`长度自动伸缩，以免字数过多显示不下。

*Solution3*：

```js
<Input addonAfter={<Button>{text}</Button>} />
```

`addonAfter` 是Input组件自带的属性。  



## 隐藏滚动条还能滚动页面

```css
body {
    width: 100vw;
    ::-webkit-scrollbar {
      display: none;
    }
  }
```



## flex-grow和flex-shrink

flex实现三栏等高布局，且两边的侧栏宽度固定，中间一栏占用剩余空间。代码如下：

```css
<style>
  section {display: flex;}
  .left-side,
  .right-side {width: 200px;}
  .content {flex-grow: 1;}
</style>
<section>
  <div class="left-side"></div>
  <div class="content"></div>
  <div class="right-side"></div>
</section>
```

三个元素会从左往右占据父元素空间，左右侧边栏的宽度都为200px，中间`.content`元素的宽度会占据`section`元素的剩余宽度。另外，`section` 的高度会自动被最高的一个子元素撑开，同时其它子元素的高度也会被拉到跟 `section` 元素一样高，而如果给 `section` 元素设置了高度，而所有子元素的高度设置为 `auto` ，所有的子元素也都会自动跟父元素一样高。

### flex-grow的计算方式（定义元素的放大比例）

上面demo中`.content`元素的`flex-grow`属性，设置为1它就可以占满剩余空间。`flex-grow`属性决定了父元素在空间分配上还有剩余空间，如何分配这些剩余空间。其值为一个权重，默认为0，剩余空间将按这个权重来分配。比如剩余空间为x，三个元素的`flex-grow`分别为a，b，c。设sum为a+b+c，那么三个元素将得到剩余空间分别为x * a / sum，x * b / sum, x * c / sum。

举个🌰：

父元素宽度 500px，三个子元素的 width 分别为 100px，150px，100px，于是剩余空间为 150px。三个元素的 `flex-grow` 分别是 1，2，3，于是 sum 为 6。则三个元素所得到的多余空间分别是：

150px * 1 / 6 = 25px

150px * 2 / 6 = 50px

150px * 3 / 6 = 75px

三个元素最终的宽度分别为 125px，200px，175px。

当所有元素的 `flex-grow `之和小于 1 的时候（注意是 1，也就是说每个元素的 `flex-grow` 都是一个小数如 0.2 ），上面式子中的 sum 将会使用 1 来参与计算，而不论它们的和是多少。也就是说，当所有的元素的 `flex-grow` 之和小于 1 的时候，剩余空间不会全部分配给各个元素。

实际上用来分配的空间是 剩余空间 * a / 1 ，剩余空间* b / 1，剩余空间 * c / 1，这些用来分配的空间依然是按 `flex-grow` 的比例来分配。

还是上面一个例子，但是三个元素的 `flex-grow` 分别是 0.1，0.2，0.3，那么计算公式将变成下面这样：

150px * 0.1 / 1 = 15px

150px * 0.2 / 1 = 30px

150px * 0.3 / 1 = 45px

150px - 15px - 30px - 45px = 60px，即还有 60px 没有分配给任何子元素。
三个元素的最终宽度分别为：

100px + 15px = 115px

150px + 30px = 180px

100px + 45px = 145px

如上所述即是 `flex-grow` 的计算方式。另外，`flex-grow` 还会受到 `max-width` 的影响。如果最终放大后的值大于 `max-width `的值，`max-width` 的值将会优先使用。同样会导致父元素有部分剩余空间没有分配。

### flex-shrink的计算方式（定义元素的缩小比例）

`flex-shrink` 属性定义空间不够时各个元素如何收缩。

举个🌰：

父元素 500px。三个子元素分别设置为 150px，200px，300px。三个子元素的 `flex-shrink`的值分别为 1，2，3。

首先，计算子元素溢出多少：150px + 200px + 300px - 500px = 150px。那这 150px 将由三个元素的分别收缩一定的量来弥补。具体的计算方式为：每个元素收缩的权重为其 flex-shrink 乘以其宽度。总权重为 1 * 150px + 2 * 200px + 3 * 300px = 1450px。

三个元素分别收缩：

150 * 1(flex-shrink) * 150px / 1450 = -15.5

150 * 2(flex-shrink) * 200px / 1450 = -41.4

150 * 3(flex-shrink) * 300px / 1450 = -93.1

三个元素的最终宽度分别为：

150px - 15.5px = 134.5px

200px - 41.4px = 158.6px

300px - 93.1px = 206.9px

当所有元素的 `flex-shrink` 之和小于 1 时，计算方式也会有所不同：此时，并不会收缩所有的空间，而只会收缩 `flex-shrink` 之和相对于 1 的比例的空间。还是上面的例子，但是 `flex-shrink`分别改为 0.1，0.2，0.3。于是总权重为 145（正好缩小 10 倍，略去计算公式）。三个元素收缩总和并不是 150px，而是只会收缩 150px 的 (0.1 + 0.2 + 0.3) / 1 即 60% 的空间：90px。

每个元素收缩的空间为：

90 * 0.1(flex-shrink) * 150px / 145 = 9.31

90 * 0.2(flex-shrink) * 200px / 145 = 24.83

90 * 0.3(flex-shrink) * 300px / 145 = 55.86

三个元素的最终宽度分别为：

150px - 9.31px = 140.69px

200px - 24.83px = 175.17px

300px - 55.86px = 244.14px

当然，类似 `flex-grow`，`flex-shrink` 也会受到 `min-width` 的影响。

**参考资料**：https://zhuanlan.zhihu.com/p/24372279

**flex实时布局效果展示**：https://demos.scotch.io/visual-guide-to-css3-flexbox-flexbox-playground/demos/



## 在项目中使用进度条

### 安装nprogress插件
```
$ yarn add nprogress
或
$ npm install --save nprogress
```

### 添加Router和NProgress的引用(在_app.js中添加)
```javascript
import NProgress from 'nprogress'
import Router from 'next/router'
//引用css样式文件
import "nprogress/nprogress.css"

Router.events.on('routeChangeStart', () => {
  NProgress.start()
})
Router.events.on('routeChangeComplete', () => NProgress.done())
Router.events.on('routeChangeError', () => NProgress.done())
```

### 在_app.js中添加样式
```javascript
<Head>
	<link rel="stylesheet" href="nprogress.css" />
</Head>
```
