# 快速开始

🚀 本文档主要记录在写前端CSS时遇到的问题及解决方法，持续更新中。

## 前端CSS命名规范

1. 没有任何理由的情况下就用worda-wordb的命名方法
2. 由具体单词组成的composable css用A-B命名，譬如float: left，就写成F-L
3. 比较复杂的常用combo，一般我们用骆驼命名法，譬如alignCenter

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

使用[Ant-Design](https://ant.design/index-cn) 的[Grid栅格系统](https://ant.design/components/grid-cn/) 

```js
<Row>
	<Col span={8} />
	<Col span={16} />
</Row>
```

## 多个div在同一行

*Question*：如何使多个div标签在同一行？

*Solution1*：
设置每个div的style样式：`display: inline-block`

使用`inline-block`同时也有问题，比如，使得`Icon`图标和一个div在同一行，但是两个行高不一样。原因是默认情况下使用`inline-block`属性时，并列显示的元素的垂直对齐方式是底部对齐，为了使垂直对齐方式改为顶部对齐，就需要在div元素的样式中加入`vertical-align`属性。

*Solution2*：
使用`display: flex`属性。

引申：
[div等块级标签横向排列的方法总结](https://blog.csdn.net/zmhawk/article/details/73293366)

[Flex布局教程-阮一峰](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html?utm_source=tuicool)

### 两个div在同一行，分别在最左边和最右边

*Solution1*:

```javascript
<div>
  <div style={{ display: 'inline-block'}}>aaa</div>
  <div style={{ float: 'right' }}>bbb</div>
</div>
```

*Solution2*:

```javascript
<div style={{ display: 'flex', justifyContent: 'flex-end'}}>
	<div style={{ display: 'flex', width: '100%'}}>aaa</div>
	<div style={{ display: 'flex'}}>bbb</div>
</div>
```

## div固定在页面底部

*Solution*：

将父div设置为`position: relative`，然后将子div样式设置为

```CSS
	position: absolute;
	bottom: 0;
```

## CSS hover效果总结

[CSS-鼠标移入移除样式改变，并有过渡效果](https://www.hangge.com/blog/cache/detail_982.html)

## div内文字超过宽度自动换行

*Solution*：

div设置宽度后style样式加上`word-break: break-all;`或`word-wrap: break-word;`

`word-break: break-all`：例如div宽度为200px，它的内容超过200px会自动换行，如果该行末尾有个很长的单词（congratulation），它会把单词截断，变成该行末端为congra，下一行为tulation。

`word-wrap: break-word`：与上面相同，但区别是它不会截断单词，它会把congratulation整个单词看成一个整体，自动把整个单词放到下一行，不会把单词截断。

## Antd修改组件样式（antd-v3）

### 1.修改Modal背景颜色

```javascript
<Modal className="modal-dialog" />
```

由于jsx不支持嵌套，所以在main.scss中添加modal-dialog样式

```css

.modal-dialog {
 .ant-modal-body {
  	background: #ffffff;
  }
}
```

### 2.修改Menu的border-right样式

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

### 3.Menu组件

`Menu`标签下不能加别的标签，否则会有warning，必须放在`<Menu.Item>...</Menu.Item>`里面。

## Antd自定义Icon图标

antd从4.0开始，不再内置Icon组件，使用独立的包`@ant-design/icons`。由于antd自带的图标不一定符合开发需求，所以有时会选择自定义图标。

本文使用的方法最简单，使用[iconfont.cn](iconfont.cn)，iconfont.cn是阿里的矢量图标库，里面有大量图标可供选择。将iconfont.cn上生成的js地址复制到代码中，即可实现渲染图标。

![iconfont](/_media/iconfont.png)

```
	import { createFromIconfontCN } from '@ant-design/icons'
	
	const MyIcon = createFromIconfontCN({
		scriptUrl: '//at.alicdn.com/t/font_1675145_7w7tb9mha85.js'
	})
	
	<MyIcon />
```



## 给项目安装webpack-bundle-analyzer

webpack-bundle-analyzer官方文档：https://www.npmjs.com/package/webpack-bundle-analyzer

通过使用webpack-bundle-analyzer可以看到项目各模块的大小，可以按需优化。

![webpack_1](/_media/webpack_1.png)

安装步骤：

1. 安装依赖包

   ```
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

要使Input输入框和Button按钮在一行.

*Solution1*：

```javascript
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

```javascript
 <Input.Search
          enterButton={text}
          value={value}
          onChange={onChange}
          onSearch={onClick}
 />
```

这种方法下，`Input`长度自动伸缩，以免字数过多显示不下。

*Solution3*：

```javascript
<Input addonAfter={<Button>{text}</Button>} />
```

`addonAfter` 是Input组件自带的属性。  

