<template>
  <scroll-view
    @scrolltolower="handleToLower"
    scroll-y
    class="home_recommend"
    v-if="recommends.length > 0"
  >
    <!-- 推荐 开始 -->
    <view class="recommend_wrap">
      <view
        class="recommend_item"
        v-for="(item, index) in recommends"
        :key="index"
        ><image :src="item.thumb" mode="widthFix"
      /></view>
    </view>
    <!-- 推荐 结束 -->
    <!-- 月份 开始 -->
    <view class="months_wrap" v-if="Object.keys(months).length > 0">
      <view class="months_title">
        <view class="months_title_info">
          <view class="months_info">
            <text>{{ months.DD }}/ </text>
            {{ months.MM }} 月
          </view>
          <view class="months_text">{{ months.title }}</view>
        </view>
        <view class="months_title_more">更多></view>
      </view>
      <view class="months_contetnt">
        <view class="months_item" v-for="item in months.items" :key="item.id">
          <image :src="item.thumb" mode="aspectFill" />
        </view>
      </view>
    </view>
    <!-- 月份 结束 -->
    <!-- 热门开始 -->
    <view class="hots_wrap">
      <view class="hots_title">
        <text>热门</text>
      </view>

      <view class="hots_content">
        <view class="hots_item" v-for="item in hots" :key="item.id">
          <image :src="item.thumb" mode="aspectFill"></image>
        </view>
      </view>
    </view>
    <!-- 热门结束 -->
    <!-- 没有更多 -->
    <nomore />
  </scroll-view>
</template>

<script>
import moment from "moment";
import nomore from "@/components/nomore";
export default {
  data() {
    return {
      // 推荐列表
      recommends: [],
      //   月份模块
      months: {},
      // 热门
      hots: [],
      // 请求参数
      params: {
        limit: 30,
        order: "hot",
        skip: 0,
      },
      // 判断接口是否还有数据
      isMore: true,
    };
  },
  async mounted() {
    this.getList();
  },
  components: {
    nomore,
  },
  methods: {
    // 获取接口数据
    async getList() {
      let data = await this.request({
        url: "http://157.122.54.189:9088/image/v3/homepage/vertical", //仅为示例，并非真实接口地址。
        data: this.params,
      });
      // 数据已经完全返回,isMore=false
      if (data.res.vertical.length == 0) {
        this.isMore = false;
        return;
      }
      // 第一次发送请求给推荐和月份赋值，以后触底重新发送请求就不给他们赋值了
      if (this.recommends.length == 0) {
        // 第一次发送请求
        // 推荐模块
        this.recommends = data.res.homepage[1].items;
        // 月份模块
        this.months = data.res.homepage[2];
        // 将时间戳 改成 18号/01月 moment.js
        this.months.MM = moment(this.months.stime).format("MM");
        this.months.DD = moment(this.months.stime).format("DD");
      }

      // 获取热门列表
      // 每次触底触发重新获取数据，数组拼接
      this.hots = [...this.hots, ...data.res.vertical];
      // console.log(data);
    },
    // 滚动条触底事件
    handleToLower() {
      /**
       * 1.修改请求参数 skip+=limit;
       * 2.重新发送请求 getList()
       * 3.请求回来成功 hots 数据叠加
       */
      if (this.isMore) {
        this.params.skip += this.params.limit;
        this.getList();
      }

      console.log("触底");
    },
  },
};
</script>

<style lang="scss" scoped>
.home_recommend {
  // 高度：屏幕高度-头部标题高度
  height: calc(100vh - 36px);
  width: 100%;
  .recommend_wrap {
    width: 100%;
    display: flex;
    flex-wrap: wrap;
    .recommend_item {
      width: 50%;
      border: 2px solid #fff;
    }
  }
  .months_wrap {
    .months_title {
      display: flex;
      justify-content: space-between;
      padding: 20rpx;
      .months_title_info {
        display: flex;
        align-items: center;
        font-weight: 600;
        .months_info {
          margin-right: 30rpx;
          color: $color;
          font-size: 30rpx;
          text {
            font-size: 36rpx;
          }
        }

        .months_text {
          font-size: 34rpx;
        }
      }

      .months_title_more {
        line-height: 48rpx;
        font-size: 24rpx;
        color: $color;
      }
    }

    .months_contetnt {
      display: flex;
      flex-wrap: wrap;
      .months_item {
        width: 33.3%;
        image {
          border: 3px solid #fff;
        }
      }
    }
  }
  .hots_wrap {
    .hots_title {
      padding: 0rpx 10rpx;
      border-left: 20rpx solid $color;
      margin: 14rpx 24rpx;
      box-sizing: border-box;
      width: 100%;
      text {
        padding-left: 10rpx;
        font-size: 32rpx;
        line-height: 21rpx;
      }
    }
    .hots_content {
      display: flex;
      flex-wrap: wrap;
      width: 100%;
      .hots_item {
        width: 33%;
        border: 5rpx solid #fff;
      }
    }
  }
}
</style>