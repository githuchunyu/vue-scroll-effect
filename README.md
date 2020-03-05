# vue-scroll-effect

一个基于vue的滚动特效插件。

目前处于学习测试阶段，暂不支持npm安装。

## 使用方法
```vue
<template>
  <ScrollView>
    <ScrollItem>
      <Block01 />
    </ScrollItem>
    <ScrollItem>
      <Block02 />
    </ScrollItem>
  </ScrollView>
<template>

<script>
import Scroll from 'vue-scroll-effect'
export default {
  components: {
    ScrollView: Scroll.ScrollView,
    ScrollItem: Scroll.Item
  }
}
</script>
```
其中ScrollView为整个滚动区域组件，ScrollItem为根据滚动作出动画效果的子组件。

## ScrollView
可以设置如下参数
### ineritia
滚动惯性，默认值为20，惯性越大，滚动停止越慢。

### speed
滚动速度，默认值为0.5，速度越快，页面跟随滚动速度越快。

### top
滚动区域离页面顶部的最大距离，默认值为0，单位px

### bottom
滚动区域离页面底部的最大距离，默认值为0，单位px

### disabled
是否禁用滚动，默认值为false

## ScrollItem
ScrollItem必须直接包裹需要做动画效果的组件，且此组件内必需有一个方法用来接受滚动的参数并作出完全自定义的响应。此方法默认为animate，参数为滚动的相对滚动距离。

### top
距离页面顶部多少距离时触发动画效果，默认为0，单位px

### once
是否只触发一次，默认为false

### method
子组件内接受滚动参数的方法名，默认为animate