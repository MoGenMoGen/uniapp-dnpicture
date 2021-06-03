<template>
  <scroll-view
    scroll-y
    @scrolltolower="handleToLower"
    @scrolltoupper="handleToUpper"
    class="home_album"
    v-if="info.banner.length > 0"
  >
    <!-- 轮播图开始 -->
    <view class="home_album_swiper">
      <swiper
        class="swiper"
        :indicator-dots="true"
        :autoplay="true"
        :interval="5000"
        :duration="500"
      >
        <swiper-item v-for="item in info.banner" :key="item.id">
          <image :src="item.thumb" mode="widthFix" />
        </swiper-item>
      </swiper>
    </view>
    <!-- 轮播图结束 -->
    <!-- 列表开始 -->
    <view class="home_album_list">
      <navigator
        class="home_album_item"
        v-for="item in info.album"
        :key="item.id"
        :url="`/pages/album/index?id=${item.id}`"
      >
        <image :src="item.cover" mode="center" />
        <view class="item_info">
          <view class="album_item_title">{{ item.name }}</view>
          <view class="album_item_content">{{ item.desc }}</view>
          <view class="album_item_attention" @click="handleAttention(item)">
            <text v-if="!item.isfav" class="attention_false">+ 关注</text>
            <text class="attention_true" v-else>已关注</text>
          </view>
        </view>
      </navigator>
    </view>
    <!-- 列表结束 -->
    <nomore />
  </scroll-view>
</template>

<script>
import nomore from "@/components/nomore";
export default {
  data() {
    return {
      // 数据
      info: {
        banner: [],
        album: [],
      },
      attention: false,
      // 请求参数
      params: {
        limit: 10,
        order: "new",
        skip: 0,
      },
      // 判断接口是否还有数据
      isMore: true,
    };
  },
  components: { nomore },
  methods: {
    async getList() {
      let data = await this.request({
        url: "http://157.122.54.189:9088/image/v1/wallpaper/album", //仅为示例，并非真实接口地址。
        data: this.params,
      });
      // 数据已经完全返回,isMore=false
      if (data.res.album.length == 0) {
        this.isMore = false;
        return;
      }
      // 第一次发送请求轮播图赋值，以后触底重新发送请求就不给他们赋值了
      if (this.info.banner.length == 0) {
        this.info.banner = data.res.banner;
      }
      this.info.album = [...this.info.album, ...data.res.album];
      console.log(data);
      console.log("banner", this.info.banner);
    },
    // 触底刷新
    handleToLower() {
      /**
       * 1.修改请求参数 skip+=limit;
       * 2.重新发送请求 getList()
       * 3.请求回来成功 hots 数据叠加
       */
      // 判断接口数据是否返回完全，有才继续
      if (this.isMore) {
        this.params.skip += this.params.limit;
        this.getList();
      }

      console.log("触底");
    },

    // 关注
    handleAttention(item) {
      item.isfav = !item.isfav;
    },
  },
  async mounted() {
    this.getList();
  },
};
</script>

<style lang="scss" scoped>
.home_album {
  // 高度：屏幕高度-头部标题高度
  height: calc(100vh - 36px);
  width: 100%;
  .home_album_swiper {
    image {
      width: 100%;
    }
  }

  .home_album_list {
    width: 100%;
    padding: 10rpx 0;
    .home_album_item {
      height: 262rpx;
      width: 100%;
      padding: 10rpx;
      box-sizing: border-box;
      border-bottom: 1px solid #ccc;
      display: flex;
      align-items: center;
      image {
        width: 250rpx;
        height: 100%;
      }
      .item_info {
        height: 100%;
        dispaly: flex;
        position: relative;
        flex-direction: column;
        justify-content: flex-start;
        flex: 1;
        box-sizing: border-box;
        .album_item_title {
          box-sizing: border-box;

          padding: 20rpx 30rpx 15rpx;
          font-size: 36rpx;
          line-height: 36rpx;
          font-weight: 600;
        }

        .album_item_content {
          padding-left: 27rpx;
          box-sizing: border-box;
          width: 100%;
          max-width: calc(100vw - 270rpx);
          overflow: hidden;
          text-overflow: ellipsis;
          white-space: nowrap;
          font-size: 28rpx;
          line-height: 28rpx;
        }

        .album_item_attention {
          position: absolute;
          bottom: 60rpx;
          right: 50rpx;
          .attention_false {
            font-size: 32rpx;
            color: $color;
            border: 1px solid $color;
          }
          .attention_true {
            font-size: 32rpx;
            color: #ccc;
            background: $color;
          }
        }
      }
    }

    nomore {
    }
  }
}
</style>