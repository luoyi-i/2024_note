
@[toc]
## CSS doc
https://www.w3schools.com/cssref/pr_class_position.php
https://www.w3schools.com/css/tryit.asp?filename=trycss_position_relative




## 微信小程序  WXSS
WXSS 具有 CSS 大部分特性.
扩展的特性：
    - 尺寸单位
    - 样式导入

###  rpx: responsive pixel. 规定屏幕宽度为 750 rpx
###  样式导入
@import后跟需要导入的外联样式表的相对路径
```
/** app.wxss **/
@import "common.wxss";
.middle-p {
  padding:15px;
}
```
### 引用外部样式
```
Component({
 externalClasses: ['menu-bg', 'menu-content', 'menu-list'], //不能使用小驼峰命名，只能用 -或 _
  properties: {
```


定义在 app.wxss 中的样式为全局样式，作用于每一个页面。
在 page 的 wxss 文件中定义的样式为局部样式，作用于对应的页面，并会覆盖 app.wxss 中相同的选择器。
 


```
wxml

```
    <view class="cls-select-container">
        <selector  data-value="{{selectId[0]}}" class="cls-selector" selectId="{{selectId[0]}}" 
        selectArray="{{selectData}}" catch:tap="catch_tap_selector"></selector>
    </view>
```


