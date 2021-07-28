# 面试题-兼容

#### IE6下元素浮动之后margin值变成双倍

``` css
// 解决方案：给该元素设置
{
  display: inline;
}
```



#### 上下 margin 重合问题

相邻的两个div margin-left margin-right 不会重合，但相邻的margin-top margin-bottom会重合。



#### td高度的问题 

 table中td的宽度都不包含border的宽度，但是oprea和ff中td的高度包含了border的高度 。

解决： 设置line-height和height一样。在ie中如果td中的没有内容，那么border将不会显示。



#### 兼容ie8 或者ie9 或任意ie版本浏览器

条件注释法

``` html
<!-->大于 gt || 大于等于 gte || 小于 lt || 小于等于 lte<-->

<!DOCTYPE html>
<!--[if IE 8 ]> <html class="ie8" lang="en"> <![endif]--> 
<!--[if IE 9 ]> <html class="ie9" lang="en"> <![endif]--> 
<!--[if (gt IE 9)|!(IE)]><!--> 
<html lang="en"> <!--<![endif]-->
```



#### 尽量少用绝对定位和浮动等高性能属性

虽然绝对定位可以很简洁的实现很棒的效果，但是由于浏览器的渲染机制，网页中如果用过多的绝对定位，会让网页加载速度变得很慢。



#### 前端页面兼容水滴屏、流海屏



#### 适配不同屏幕大小



#### 移动端1像素问题

