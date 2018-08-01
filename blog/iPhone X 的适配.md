# iPhone X 的适配

> iPhone x 一出来因为有刘海受到了外形上的争议，作为一枚前端看到的是 适配问题。


### iPhone X 数据
各iphone设备分辨率，retina值见下图
![iphone-excel](https://raw.githubusercontent.com/lixiaraodd01/lisa/master/images/device-excel.jpg)


### iPhone X 新概念 安全区域 (safe-area)

iPhone X除去刘海和下面的操作栏还有margin区域就是安全区域，如下图蓝色区域。
![iphone x安全区域](https://raw.githubusercontent.com/lixiaraodd01/lisa/master/images/iphone-x-safe-area.png)


### iPhone X 适配方法

h5页面为了刘海屏不影响显示，下面操作栏不影响操作所以需要做适配。

适配方式有3种

1. 使用媒体查询(media query),对于iphone x 写个`iphone-x.css`文件

```css
iphone-x.css

@media only screen 
    and (device-width : 375px) 
    and (device-height : 812px) 
    and (-webkit-device-pixel-ratio : 3) {
      /*需要特殊处理的部分*/
      /*header*/
      .has-x-header {
        padding-top: 44px; /*刘海屏高度为44px*/
      }

      /*footer*/
      .has-x-footer {
        padding-bottom: 34px; /* iphone x 下面操作一横的高度为34px*/
      }

    }
```
2. webview 客户端处理, 可以参照腾讯团队通过协商规则进行处理。

具体是通过链接中增加参数来进行适配:

- 参数名:_wvx 控制 iPhone X 适配行为
- 参数名:_wvxTclr 控制顶部适配层颜色
- 参数名:_wvxBclr 控制底部适配层颜色

3. 还是通过样式方式，但不需要另外写样式文件。使用`css`新属性。方法如下

- 首先在html文件中设置`meta`

```html
<meta name="viewport" content="viewport-fit=cover">
```
`viewport-fit` 可设置3个值，解释如下

```
cover = 全部覆盖 网页内容完全覆盖可视窗口
contain = 网页内容完全覆盖可视窗口
auto =  contain
```

*注意: 这个设置在UIWebview和 WKWebview 中都生效。如果不生效则是因客户端设置问题。`UIScrollViewContentInsetAdjustmentNever`方法的设置。*

- 其次在样式文件中使用css新属性`constant` 和 `env`.

```css
header {
    /* Status bar height on iOS 10 */
    padding-top: 44px;
    /* Status bar height on iOS 11.0 */
    padding-top: constant(safe-area-inset-top);
    /* Status bar height on iOS 11+ */
    padding-top: env(safe-area-inset-top);
}
```
关于`safe-area-inset-xxx`如下图

![safe-area-inset-xx](https://raw.githubusercontent.com/lixiaraodd01/lisa/master/images/iphone-x-safe-area-constant.png)

`safe-area-inset-xx`可以用在`margin` `padding` `top` `bottom`值中。遇到支持的就会覆盖掉上面的值。


