<template>
  <!-- 专辑详情页 -->
  <view class="page_album" v-if="Object.keys(album).length != 0">
    <!-- 专辑背景 开始 -->
    <view class="album_bg">
      <image :src="album.cover" mode="" />
      <view class="album_info">
        <view class="album_name">{{ album.name }}</view>
        <view class="album_btn">关注专辑</view>
      </view>
    </view>
    <!-- 专辑背景 结束 -->
    <!-- 用户信息和专辑描述 开始 -->
    <view class="album_detail">
      <view class="userinfo">
        <image :src="album.user.avatar" mode="" />
        <text>{{ album.user.name }}</text>
      </view>
      <view class="album_desc">
        {{ album.desc }}
      </view>
    </view>
    <!-- 用户信息和专辑描述 结束 -->
    <!-- 专辑图片列表 开始 -->
    <view class="album_img_list">
      <view
        class="album_img_item"
        v-for="(item, index) in wallpaper"
        :key="item.id"
      >
        <go-img :list="wallpaper" :index="index">
          <image :src="item.img" mode="" />
        </go-img>
      </view>
    </view>
    <!-- 专辑图片列表 结束 -->
    <nomore />
  </view>
</template>

<script>
import goImg from "@/components/goImg";
import nomore from "@/components/nomore";
export default {
  data() {
    return {
      params: {
        limit: 10,
        order: "new",
        skip: 0,
        // 1 返回值中有 album 对象 表示第一次请求数据
        // 0 返回值中 只有 wallpaper 数组 不是第一次请求
        first: 1,
      },
      //   接口中是否还有数据可以请求
      isMore: true,
      id: -1,
      album: {}, //专辑详情
      wallpaper: [], //"专辑"列表
    };
  },
  onLoad(options) {
    this.id = options.id;
    this.getList();
    console.log(options);
  },
  //   触底事件
  onReachBottom() {
    if (this.isMore) {
      this.params.first = 0;
      this.params.skip += this.params.limit;
      this.getList();
    }
  },
  methods: {
    async getList() {
      let data = await this.request({
        url: `http://157.122.54.189:9088/image/v1/wallpaper/album/${this.id}/wallpaper`, //仅为示例，并非真实接口地址。
        data: this.params,
      });
      //   第一次请求给 专辑详细信息album赋值
      if (this.params.first == 1) this.album = data.res.album;
      // 如果接口返回数组为空，则isMore=false
      if (data.res.wallpaper.length == 0) {
        this.isMore = false;
        return false;
      }
      this.wallpaper = [...this.wallpaper, ...data.res.wallpaper];
    },
  },

  components: { nomore, goImg },
};
</script>

<style lang="scss" scoped>
.page_album {
  width: 100%;
  .album_bg {
    position: relative;

    width: 100%;
    image {
      width: 100%;
      height: 340rpx;
    }

    .album_info {
      width: 100%;
      position: absolute;
      bottom: 20rpx;
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding-left: 30rpx;
      padding-right: 30rpx;
      .album_name {
        color: #fff;
        font-size: 40rpx;
      }

      .album_btn {
        background: $color;
        color: #fff;
        font-size: 32rpx;
        padding: 10rpx;
      }
    }
  }
  .album_detail {
    width: 100%;
    padding: 20rpx;
    padding-bottom: 50rpx;
    .userinfo {
      margin-bottom: 15rpx;
      display: flex;
      align-items: center;
      image {
        margin-right: 20rpx;
        width: 55rpx;
        height: 55rpx;
      }

      text {
        font-weight: 600;
        font-size: 40rpx;
        color: #101310;
      }
    }

    .album_desc {
      font-size: 34rpx;
      line-height: 48rpx;
    }
  }
  .album_img_list {
    width: 100%;
    display: flex;
    flex-wrap: wrap;
    .album_img_item {
        width: 33%;
      border: 1px solid #fff;
      box-sizing: border-box;
      image {
        height: 250rpx;
      }
    }
  }
}
</style>