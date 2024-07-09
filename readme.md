# Taro 微信小程序 markdown 渲染组件

##感谢 

GitHub地址：https://github.com/TooBug/wemark 提供原始方案；

最近在做一个单词阅读的小程序，需要 `markdown` 渲染数据，目前有很多方案无法使用，于是修改了原作者的代码。

## 使用方法

1. 把 wemark 下的文件复制到自己项目中；
2. 在项目像组件一样引用就可以了。
```javascript
<script lang="ts" setup>
import Wemark from '@/component/wemark/Wemark.vue'
</script>
```
```html
<Wemark :md="item.content" type="wemark" link highlight />
```

## 效果
![](/result.jpg)

## Buy me a coffee

![](/IMG_2237.JPG)
