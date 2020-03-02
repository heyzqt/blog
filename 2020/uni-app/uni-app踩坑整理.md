# 知识点整理

# 2020-02-21
## 1.实际转换rpx值转换成px值会偏小，最好px值+1后再进行转换

## 2.将子组件封装进components中后，采用标签image，并使用src引入本地图片，图片读取失败，最好使用img标签

## 3.vue的v-show标签可能在小程序为hidden却失效的情况
+ 网上说的原因是以下2点，但我实际测试都这两种原因情况下，hidden也能生效，所以实际原因还未知
    + 原因1：hidden只作用于块状布局才生效，也就是display:block;
    + 原因1：display:flex;导致hidden失效
+ 解决办法1：（不推荐）
    + 使用v-if代替v-show
+ 解决办法2：（推荐）
    + 可以在v-show的组件之外再多加一层view，在这层view上使用v-show
+ 解决办法3：（不推荐)
    + 在组件中使用变量控制display:none显示来控制组件的显示隐藏
        + 实际我使用了以下代码块测试，但是在HBuilder中编译微信小程序时，报错失败，原因不识别该语法
        ```
        :style="`display: ${tabIndex==0 ? 'block':'none'}`"
        ```
## 4.使用$attrs获取不到父组件向子组件传的值
+ 可以通过`this.$root.customValue`获取到
+ 可以通过`props`传递获取

## 5.width:100%; 可能会导致margin-right、padding-left、padding-right无效，margin-left有效，元素右侧撑满的情况，去掉width: 100%;即可,