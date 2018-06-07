# css  in module

### 很长的前言

以前一直捣鼓`css`从预处理器`sass`、`compass`、`lesss`。一直想着怎么能把`css`有模块化和方法继承方面靠。
后面大略了解了一下后处理器`postcss`。预处理器和后处理器都需要依赖构建工具最终还是生产css文档。但在写的时候就简便多了。最近草草看了一下`webpack4`,跟着文档配置了一下，后发现居然可以直接在`css-loader`中配置`module`。类名成了hash形式。

webpack 配置片段
```js
  {
    test: /\.css$/,
    use: [
      {
        loader: "style-loader"
      },
      {
        loader: "css-loader",
        options: {
          modules: true,
          importLoaders: 1,
          localIdentName: "[name]_[local]_[hash:base64]",
          sourceMap: true,
          minimize: true
        }
      }
    ]
  }
```

index.css

```css
.button {
  display: block;
  height: 32px;
  margin: 20px auto;
  line-height: 32px;
  text-align: center;
  padding: 10px 15px;
  border-radius: 10px;
  background-color: pink;
  color: #fff;
  font-size: 12px;
  cursor: pointer;
}
```

react文件中使用

```js
import styles from './index.css';
<a key="propschange-button" className={styles.button} onClick={this.propsChange.bind(this)}>propschange</a>
```

编译后会将类名按配置 `[name]_[local]_[hash:base64]`的规则生产。所以看到的类名就如下图。

![css in module类名唯一](https://raw.githubusercontent.com/lixiaraodd01/lisa/master/images/css_in_module.jpg)


### 什么是 css in module
按module的css。把css分了局部和全局的概念依赖在构建工具上的一个方式。即

css文档中书写

```css
:local(.test) {
  color: #f00;
}

:global(.test) {
  color: green;
}
```
组合基础

```css
.className {
  background-color: blue;
}

.title {
  composes: className;
  color: red;
}
```

```css
.title {
  composes: className from './another.css';
  color: red;
}
```

app.js中引用和上面一样

```jsx
import style from './index.css';
<div className={style.title}>
```

最后生产的css文件和html的class名都会根据配置生成带hash的形式。如图片(css in module类名唯一)



### css in module 有什么好处
解决了以下几点问题
- 全局污染

CSS 使用全局选择器机制来设置样式，优点是方便重写样式。缺点是所有的样式都是全局生效，样式可能被错误覆盖，因此产生了非常丑陋的 !important，甚至 inline !important 和复杂的选择器权重计数表，提高犯错概率和使用成本。Web Components 标准中的 Shadow DOM 能彻底解决这个问题，但它的做法有点极端，样式彻底局部化，造成外部无法重写样式，损失了灵活性。

- 类名混乱

由于全局污染的问题，多人协同开发时为了避免样式冲突，选择器越来越复杂，容易形成不同的命名风格，很难统一。样式变多后，命名将更加混乱。

- 无法共享变量

less或者sass都无法跨文件使用变量


### css in module有什么坏处
有以下几点劣势
- 不能使用选择器，只使用 class 名来定义样式。因为它只对class和id能hash到，选择器不行。
- 不能使用多个class来写样式，不然违背了这个意义
- 不能嵌套
- 老项目切换。每个组件需要引入多个样式文件，配置没配置好，后期维护非常麻烦。类名不能直接定位。


### 个人对css in module的看法

个人感觉`css in module`是一种构建工具上的便利，在这便利上约束写css的规范。对于不能使用多个class，嵌套等。这和css原本的概念完全背驰了。不推荐使用在大型业务开发。如是开源项目或者公共组件等可以试用。































