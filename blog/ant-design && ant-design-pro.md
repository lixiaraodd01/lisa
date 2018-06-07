
# ant-design && ant-design-pro

### ant-design
`ant-design`是一套组件库。由蚂蚁金服设计统一的一套规范提供的一套react基础组件库。


#### 商业后台使用ant-design心得

用到的组件有

``` js
Button // 按钮 可通过属性设置
Icon  // 图标 (有点鸡肋)
Grid  // 栅格化(这个被乱用的最严重, 通常gutter设置后然后用样式又复写固定死。也有直接用flex，然后混写。)
Layout  // 布局
Affix   // 图钉 固定在页面某个位置 
Breadcrumb  // 面包屑，这个配合路由来设置。
Menu  // 菜单
Pagination  //分页
Steps // 步骤条
Checkbox  // 复选框
DatePicker  // 日历 (使用时需要和monent插件一起用)
Form  // 表单
Input // 单行文本框
Radio // 单选
Select  //  选择器
Upload  // 上传
Card  // 卡片 (鸡肋)
Popover // 气泡卡 (效果一般般)
Tooltips // 文字提示
Table   // 表格
Modal   // 对话框 ()
Tree    // 树
Message // 全局提示(message tips)
```


因多人开发，多人设计。导致从设计没统一规范，到前端每个人都写自己的业务模块。后面再统一规范就出现大量的样式复写和改写。感觉违背了插件本身的初衷。(笑)

`ant-design`大多数都是基础显示组件，一些无交互性的建议要么统一规范，共用组件在component中，要么直接用原html DOM，更优雅简洁。


### ant-design-pro

`ant-design-pro` 是一个企业级中后台前端/设计解决方案，也就是一套拥有基础功能的后台源码。可做二次开发。


暂时没有进行二次开发，下次补上。





































