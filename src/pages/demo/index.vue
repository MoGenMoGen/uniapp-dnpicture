<template>
  <view
  @touchstart="handleTouchstart"
  @touchend="handleTouchend"
  >
      学习触屏事件
  </view>
</template>

<script>
export default {
    data() {
        return {
            // 按下的时间
            startTime:0,
            // 按下的坐标
            startX:0,
            startY:0,
            // 离开的时间
            // endTime:0,
            // 离开的坐标
            // endX:0,
            // endY:0,
        }
    },
methods:{
    handleTouchstart(event){
        this.startX=event.changedTouches[0].clientX;
        this.startY=event.changedTouches[0].clientY;
        this.startTime=Date.now();
    
    
        // console.log("手指按下屏幕");
        // console.log(this.startX,this.startY,this.startTime);

    },
    handleTouchend(event){
        const endX=event.changedTouches[0].clientX;
        const endY=event.changedTouches[0].clientY;
        const endTime=Date.now();
        
        // 判断按下的时长
        if(endTime-this.startTime>2000)
        return

        // 滑动的方向
        let direction="" 
        // 先判断用户滑动的距离 是否合法 合法：判断滑动的方向 注意：距离要加上绝对值
        if(Math.abs(endX-this.startX)>10)
        {
            // 滑动方向！！！
            direction=endX-this.startX>0?"right":"left";
        }
        else{
            return
        }
        // 用户做了合法的滑动操作
        console.log(direction);

        // console.log("手指离开屏幕");
        // console.log(endX,endY,endTime);
        // console.log("时间差",endTime-this.startTime);
    }
}
}
</script>

<style>
view{
    width: 100%;
    height: 500rpx;
    background: cyan;
}
</style>