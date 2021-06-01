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
      <image :src="imgDetail.thumb" mode="widthFix" />
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
  </view>
</template>

<script>
import moment from "moment";
// 设置语言为中文
moment.locale("zh-cn");
export default {
  data() {
    return {
      // 图片信息对象 包含用户头像
      imgDetail: {},
    //   专辑数据 数组
    album:[],
    // 最热评论
    hot:[],
    // 最新评论
    comment:[]
    };
  },
  onLoad() {
    //   获取列表和下标
    const { imgList, imgIndex } = getApp().globalData;
    this.imgDetail = imgList[imgIndex];
    
    //   将时间戳改为几天前
    this.imgDetail.cnTime = moment(this.imgDetail.atime * 1000).fromNow();
    // 获取图片详情的id
    // this.imgDetail.id
    this.getComments(this.imgDetail.id);
    // console.log(this.imgDetail);
  },
  methods: {
    //   获取图片详情所有评论
    async getComments(id) {
      let data = await this.request({
        url: `http://157.122.54.189:9088/image/v2/wallpaper/wallpaper/${id}/comment`,
      });
      this.album=data.res.album;
      this.hot=data.res.hot;
      this.comment=data.res.comment;

      console.log(data);
    },
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
      image{
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
}
</style>