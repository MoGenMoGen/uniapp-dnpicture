<template>
  <view class="video_play">
    <!-- <image :src="videoObj.video" mode="" /> -->
    <!-- 工具栏 开始 -->
    <view class="video_tool">
      <view class="iconfont iconshoucang" @click.stop="handleshoucang"> </view>
      <view
        :class="['iconfont', muted ? 'iconjingyin' : 'iconshengyin']"
        @click.stop="muted = !muted"
      ></view>
      <view class="iconfont iconzhuanfa">
        <button open-type="share"></button>
      </view>
    </view>
    <!-- 工具栏 结束 -->
    <!-- 视频 开始 -->
    <view class="video_wrap">
  <swiper-action @swiperAction="handleSwiperAction" :isVertical=true>
       <!-- 拉伸内容属性 -->
      <video
        :src="videoObj.video"
        :controls="false"
        :muted="muted"
        loop="true"
        :enable-play-gesture="true"
        objectFit="fill"
        autoplay="true"
      ></video>  
      </swiper-action>
     
    </view>

    <!-- 视频 结束 -->
    <!-- 下载 开始 -->
    <!-- <view class="download">
      <view class="download_btn">下载高清</view>
    </view> -->
    <!-- 下载 结束 -->
  </view>
</template>

<script>
import swiperAction from "@/components/swiperAction";
export default {
  data() {
    return {
      videoObj: {},
      muted: false,
      videoIndex:0,
      videoList:[]

    };
  },
  onLoad() {
    // this.videoObj = getApp().globalData.video;
      const { videoIndex, videoList } = getApp().globalData;
    this.videoIndex = videoIndex;
    this.videoList = videoList;
   this.videoObj=this.videoList[this.videoIndex];
   console.log("aaaa",this.videoIndex,this.videoList);
  },
  methods: {
    async handleshoucang() {
      uni.showLoading({
        title: "下载中",
      });
      // 1.将远程文件 下载到小程序内存中
      let data = await uni.downloadFile({
        url: this.videoObj.video,
      });
      const { tempFilePath } = data[1];
      // 2.将内存中的文件 下载到本地
      uni.saveVideoToPhotosAlbum({
        filePath: tempFilePath,
        success() {
          uni.hideLoading();
          uni.showToast({
            title: "下载成功",
            icon: "success",
          });
        },
        fail(err) {
          console.log(err);
          uni.hideLoading();
          uni.showToast({
            title: "下载失败",
            icon: "error",
          });
        },
      });
    },
    handleSwiperAction(e){
      console.log(e);
           /**
       * 1.用户 上滑 this.videoIndex++
       * 2.用户 下滑 this.videoIndex--
       * 3.判断 数组是否越界问题
       * 4.上滑 e.direction=="up"&&this.videoIndex<videoList.length-1 可以上滑加载下一页
       * 5.下滑 e.direction=="down"&&this.videoIndex>0 可以下滑 this.videoIndex--
       */
      if (e.direction == "up" && this.videoIndex < this.videoList.length - 1) {
        // 可以进行左滑加载下一页
        this.videoIndex++;
        
      } else if (e.direction == "down" && this.videoIndex > 0) {
        this.videoIndex--;
      
      } else {
        uni.showToast({
          title: "没有数据了",
          icon: "none",
          duration: 2000,
        });
      }
      this.videoObj=this.videoList[this.videoIndex]
    }
  },
   components: {
    swiperAction,
  },
};
</script>

<style lang="scss" scoped>
.video_play {
  width: 100vw;
  height: 100vh;
  position: relative;
  image {
    position: absolute;
    width: 100vw;
    height: 100vh;
    z-index: -1;
    // css3滤镜
    filter: blur(3px);
  }
  .video_tool {
    z-index: 1000;
    position: absolute;
    top: 0;
    right: 0;
    height: 80rpx;
    display: flex;
    align-items: center;
    justify-content: flex-end;
    width:300rpx ;
    .iconfont {
      height: 80rpx;
      width: 80rpx;
      color: #fff;
      border-radius: 50%;
      font-size: 50rpx;
      background-color: rgba(0, 0, 0, 0.2);
      display: flex;
      justify-content: center;
      align-items: center;
      margin-right: 20rpx;
    }
    .iconzhuanfa {
      position: relative;
      button {
        position: absolute;
        width: 100%;
        height: 100%;
        opacity: 0;
      }
    }
  }

  .video_wrap {
    // display: flex;
    // justify-content: center;
    video {
      width: 100vw;
      height: 100vh;
      // height: 600rpx;
    }
  }

  .download {
    display: flex;
    justify-content: center;
    .download_btn {
      background: rgba(0, 0, 0, 0.7);
      width: 80%;
      height: 80rpx;
      border-radius: 40rpx;
      display: flex;
      justify-content: center;
      align-items: center;
      color: #fff;
      border: 3px solid #fff;
    }
  }
}
</style>