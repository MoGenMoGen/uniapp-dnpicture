<template>
  <!-- 图片详情页 -->
  <view class="page_imgdetail">
    <!-- 用户信息 开始 -->
    <view class="user_info">
      <view class="user_icon">
        <image :src="imgDetail.user.avatar" mode="widthFix"></image>
      </view>
      <view class="user_desc">
        <view class="user_name">{{ imgDetail.user.name }}</view>
        <view class="user_time">{{ imgDetail.cnTime }}</view>
      </view>
    </view>
    <!-- 用户信息 结束 -->
    <!-- 高清大图 开始 -->
    <view class="high_img">
      <swiper-action @swiperAction="handleSwiperAction">
        <image :src="imgDetail.thumb" mode="widthFix" />
      </swiper-action>
    </view>
    <!-- 高清大图 结束 -->
    <!-- 点赞 开始 -->
    <view class="user_rank">
      <view class="rank">
        <text class="iconfont icondianzan">{{ imgDetail.rank }}</text>
      </view>
      <view class="user_collect">
        <text class="iconfont iconshoucang">收藏</text>
      </view>
    </view>
    <!-- 点赞 结束 -->
    <!-- 下载 开始 -->
    <view class="download">
      <text class="download_btn" @click="handleDownload">下载图片</text>
    </view>
    <!-- 下载 结束 -->
  </view>
</template>

<script>
import swiperAction from "@/components/swiperAction";
import moment from "moment";
// 设置语言为中文
moment.locale("zh-cn");
export default {
  data() {
    return {
      // 图片信息对象 包含用户头像
      imgDetail: {},
      //   专辑数据 数组
      album: [],
      // 最热评论
      hot: [],
      // 最新评论
      comment: [],
      // 图片索引
      imgIndex: 0,
      // 图片列表
      imgList: [],
    };
  },
  onLoad() {
    //   获取列表和下标
    const { imgIndex, imgList } = getApp().globalData;
    this.imgIndex = imgIndex;
    this.imgList = imgList;
    this.getData();
  },
  methods: {
    // 给当前页面赋值
    getData() {
      this.imgDetail = this.imgList[this.imgIndex];

      //   将时间戳改为几天前
      this.imgDetail.cnTime = moment(this.imgDetail.atime * 1000).fromNow();
      // 获取图片详情的id
      // this.imgDetail.id
      this.getComments(this.imgDetail.id);
      // console.log(this.imgDetail);
    },
    //   获取图片详情所有评论
    async getComments(id) {
      let data = await this.request({
        url: `http://157.122.54.189:9088/image/v2/wallpaper/wallpaper/${id}/comment`,
      });
      this.album = data.res.album;
      this.hot = data.res.hot;
      this.comment = data.res.comment;

      console.log(data);
    },
    // 滑动事件
    handleSwiperAction(e) {
      /**
       * 1.用户 左滑 this.imgIndex++
       * 2.用户 右滑 this.imgIndex--
       * 3.判断 数组是否越界问题
       * 4.左滑 e.direction=="left"&&this.imgIndex<imgList.length-1 可以左滑加载下一页
       * 5.右滑 e.direction=="right"&&this.imgIndex>0 可以右滑 this.imgIndex--
       */
      if (e.direction == "left" && this.imgIndex < this.imgList.length - 1) {
        // 可以进行左滑加载下一页
        this.imgIndex++;
        this.getData();
      } else if (e.direction == "right" && this.imgIndex > 0) {
        this.imgIndex--;
        this.getData();
      } else {
        uni.showToast({
          title: "没有数据了",
          icon: "none",
          duration: 2000,
        });
      }
    },
    // 点击下载图片
    async handleDownload() {
      await uni.showLoading({
        title: "下载中",
      });
      //  uni.downloadFile 文件下载到小程序内存
      // uni.saveImageToPhotosAlbum 文件下载到本地

      // 1.将远程文件下载到小程序的内存中 tempFilePath
      const result1 = await uni.downloadFile({
        url: this.imgDetail.img,
      });
      const { tempFilePath } = result1[1];
      // 2. 将小程序内存中的临时文件下载到本地上
      const result2 = uni.saveImageToPhotosAlbum({
        filePath: tempFilePath,
        success() {
          uni.showToast({
            title: "下载成功",
          });
        },
        fail() {
          uni.showToast({
            title: "下载失败",
          });
        },
        complete() {
          uni.hideLoading();
        },
      });
    },
  },
  components: {
    swiperAction,
  },
};
</script>

<style lang="scss" scoped>
.page_imgdetail {
  width: 100%;

  .user_info {
    display: flex;
    padding: 20rpx;
    .user_icon {
      padding: 0 20rpx;
      image {
        width: 88rpx;
        border-radius: 50%;
      }
    }

    .user_desc {
      .user_name {
        color: #000;
        font-weight: 600;
      }

      .user_time {
        color: #ccc;
        font-size: 24rpx;
        padding: 10rpx 0;
      }
    }
  }
  .high_img {
    image {
      border-bottom: 5rpx solid #eee;
    }
  }
  .user_rank {
    border-bottom: 5rpx solid #eee;
    height: 80rpx;
    display: flex;
    .rank {
      display: flex;
      justify-content: center;
      align-items: center;
      flex: 1;
      .iconfont {
      }
    }

    .user_collect {
      flex: 1;
      display: flex;
      justify-content: center;
      align-items: center;
      .iconfont {
      }
    }
  }
  .download {
    height: 120rpx;
    display: flex;
    align-items: center;
    justify-content: center;
    .download_btn {
      width: 90%;
      height: 80%;
      background-color: $color;
      color: #fff;
      font-size: 50rpx;
      font-weight: 600;
      display: flex;
      justify-content: center;
      align-items: center;
    }
  }
}
</style>