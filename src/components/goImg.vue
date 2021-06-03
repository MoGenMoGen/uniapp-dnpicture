<template>
  <!-- 
    超链接组件
 -->
  <view @click="handleClick">
    <slot> </slot>
  </view>
</template>

<script>
export default {
  props: {
    list: {
      type: Array,
      default() {
        return [];
      },
    },
    index: {
      type: Number,
      default() {
        return 0;
      },
    },
    isvideo: {
      type: Boolean,
      default() {
        return false;
      },
    },
  },
  methods: {
    handleClick() {
      if (!this.isvideo) {
        // 1.将数据缓存下来
        getApp().globalData.imgList = this.list;
        getApp().globalData.imgIndex = this.index;

        // 2.再实现点击跳转页面
        uni.navigateTo({
          url: "/pages/imgDetail/index",
        });
      }
      else{
           // 1.将数据缓存下来
        getApp().globalData.videoList = this.list;
        getApp().globalData.videoIndex = this.index;

        // 2.再实现点击跳转页面
        uni.navigateTo({
          url: "/pages/videoplay/index",
        });
      }
    },
  },
};
</script>

<style lang="sass" scoped>
</style>