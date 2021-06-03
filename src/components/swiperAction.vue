/*
手势滑动组件
1.slot
2. 对外提供数据 滑动的方向
 */
<template>
  <cover-view @touchstart="handleTouchstart" @touchend="handleTouchend">
    <slot></slot>
  </cover-view>
</template>

<script>
export default {
  props: {
    isVertical: false, //横向还是竖向的滑动组件
  },
  data() {
    return {
      // 按下的时间
      startTime: 0,
      // 按下的坐标
      startX: 0,
      startY: 0,

      // 离开的时间
      // endTime:0,
      // 离开的坐标
      // endX:0,
      // endY:0,
    };
  },
  methods: {
    handleTouchstart(event) {
      this.startX = event.changedTouches[0].clientX;
      this.startY = event.changedTouches[0].clientY;
      this.startTime = Date.now();
    },
    handleTouchend(event) {
      const endX = event.changedTouches[0].clientX;
      const endY = event.changedTouches[0].clientY;
      const endTime = Date.now();

      // 判断按下的时长
      if (endTime - this.startTime > 5000) return;

      // 滑动的方向
      let direction = "";
      if (!this.isVertical) {
        // 先判断用户滑动的距离 是否合法 合法：判断滑动的方向 注意：距离要加上绝对值
        if (
          Math.abs(endX - this.startX) > 10 &&
          Math.abs(endY - this.startY) < 10
        ) {
          // 滑动方向！！！
          direction = endX - this.startX > 0 ? "right" : "left";
        } else {
          return;
        }
      }
      else{
        // 先判断用户滑动的距离 是否合法 合法：判断滑动的方向 注意：距离要加上绝对值
        if (
          Math.abs(endY - this.startY) > 10 &&
          Math.abs(endX - this.startX) < 10
        ) {
          // 滑动方向！！！
          direction = endY - this.startY > 0 ? "down" : "up";
        } else {
          return;
        }
      }

      // 用户做了合法的滑动操作
      this.$emit("swiperAction", { direction });
    },
  },
};
</script>

<style lang="scss" scoped>
</style>