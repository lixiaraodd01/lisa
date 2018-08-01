# webview 页面调试

> 有时候直接在chrome中开启设备开发，到真机上也还是会有各种小问题，那么就需要来调试。分iphone设备和android设备。

> 这次需求中引入第三方聊天框，在不同设备上显示有些差异，未解决这些问题，进行了真机调试。 

下面说说方法：

1. 安卓设备调试。

    - 首先使用数据线将安卓设备和windows电脑上连接。
    - 在安卓设备中的设置中设置允许开启开发者模式
    - windows电脑显示设备已连接
    - 在windows chrome浏览器地址栏输入`chrome://inspect/#devices`，安卓设备在移动端chrome中输入需要调试的页面地址，然后就会在windows chrome中见到如下图
    ![inspect中设备已连接](https://raw.githubusercontent.com/lixiaraodd01/lisa/master/images/android-inspect-2.jpg)

    然后在点开 inspect 可以直接进行调试，如下图
    ![](https://raw.githubusercontent.com/lixiaraodd01/lisa/master/images/android-inspect-1.jpg)


2. iphone设备使用mac safari进行调试

    - 在mac中 运行Safari,点击“Safari”菜单下面的“偏好设置（Preferences...）”，切换到“高级选项（Advanced）”：
    ![](https://raw.githubusercontent.com/lixiaraodd01/lisa/master/images/mac-safari-setting.jpg)
    - mac safari可以看到有个 `开发`的tab
    ![](https://raw.githubusercontent.com/lixiaraodd01/lisa/master/images/safari-develop.jpg)
    - 用数据线把iphone设备和mac连接起来就可以在 `开发`选项中看到看到已连接的设备
    - 在iphone设备中使用safari浏览器打开需要调试的页面，然后在mac safari中的 开发进行调试。


3. weinre 工具调试

    - 安装weinre工具

      ```bash
      npm install -g weinre
      ```
    - 接着执行 
      ```bash
      weinre --boundHost -all-
      ```
      如下截图
    ![](https://raw.githubusercontent.com/lixiaraodd01/lisa/master/images/weinre-bash-1.jpg)

    - 然后在浏览器中打开 `http://localhost:8080/`, 出现下面页面

      ![](https://raw.githubusercontent.com/lixiaraodd01/lisa/master/images/weinre-2.png)

    - 点击进入调试

      ![](https://raw.githubusercontent.com/lixiaraodd01/lisa/master/images/weinre-debug.jpg)
    
    *需要在html页面插入`<script src="http://10.11.1.61:8086/target/target-script-min.js#anonymous"></script>`这段代码*







