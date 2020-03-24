# 知识点整理

# 2020-02-21

## 1.实际转换 rpx 值转换成 px 值会偏小，最好 px 值+1 后再进行转换

## 2.将子组件封装进 components 中后，采用标签 image，并使用 src 引入本地图片，图片读取失败，最好使用 img 标签

## 3.vue 的 v-show 标签可能在小程序为 hidden 却失效的情况

- 网上说的原因是以下 2 点，但我实际测试都这两种原因情况下，hidden 也能生效，所以实际原因还未知
  - 原因 1：hidden 只作用于块状布局才生效，也就是 display:block;
  - 原因 1：display:flex;导致 hidden 失效
- 解决办法 1：（不推荐）
  - 使用 v-if 代替 v-show
- 解决办法 2：（推荐）
  - 可以在 v-show 的组件之外再多加一层 view，在这层 view 上使用 v-show
- 解决办法 3：（不推荐)
  - 在组件中使用变量控制 display:none 显示来控制组件的显示隐藏
    - 实际我使用了以下代码块测试，但是在 HBuilder 中编译微信小程序时，报错失败，原因不识别该语法
    ```
    :style="`display: ${tabIndex==0 ? 'block':'none'}`"
    ```

## 4.使用\$attrs 获取不到父组件向子组件传的值

- 可以通过`this.$root.customValue`获取到
- 可以通过`props`传递获取

## 5.width:100%; 可能会导致 margin-right、padding-left、padding-right 无效，margin-left 有效，元素右侧撑满的情况，去掉 width: 100%;即可

## 6.页面间通信，uni.navigateTo()url 传值，传值不全问题

- 问题
  - 从 A 页面使用 uni.navigateTo()跳到 B 页面，url 中带了 1 个 json 参数，但是在 B 页面接收到的 json 被截取了
- 原因
  - url 传值，值中如果有=或&符号，字符串内容会被截取
- 解决办法
  - 使用 localstorage 保存值传值，最好不用url传这么长的值
