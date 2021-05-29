<template>
  <view class="home_recommend">
    <!-- 推荐 开始 -->
    <view class="recommend_wrap" v-if="recommends.length>0">
      <view
        class="recommend_item"
        v-for="(item, index) in recommends"
        :key="index"
        ><image :src="item.thumb" mode="widthFix"
      /></view>
    </view>
    <!-- 推荐 结束 -->
    <!-- 月份 开始 -->
    <view class="months_wrap" v-if="Object.keys(months).length>0">
      <view class="months_title">
        <view class="months_title_info">
          <view class="months_info">
            <text>{{months.DD}}/ </text>
            {{months.MM}} 月
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
  </view>
</template>

<script>
import moment from "moment";
export default {
  data() {
    return {
      // 推荐列表
      recommends: [],
      //   月份模块
      months: {},
      //   月份列表
    //   monthsbox: [],
      //   标题
    //   monthstitle: "你负责美丽就好",
    };
  },
  async mounted() {
    let data = await this.request({
      url: "http://157.122.54.189:9088/image/v3/homepage/vertical", //仅为示例，并非真实接口地址。
      data: {
        limit: 100,
        order: "hot",
        skip: 0,
      },
    });
    // 推荐模块
    this.recommends = data.res.homepage[1].items;
    // 月份模块
    this.months = data.res.homepage[2];
    // 将时间戳 改成 18号/01月 moment.js
    this.months.MM = moment(this.months.stime).format("MM");
    this.months.DD = moment(this.months.stime).format("DD");
    console.log("月份模块", this.months);
    
    console.log(data);
  },
};
</script>

<style lang="scss" scoped>
.home_recommend {
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
          border: 1px solid #fff;
        }
      }
    }
  }
}
</style>