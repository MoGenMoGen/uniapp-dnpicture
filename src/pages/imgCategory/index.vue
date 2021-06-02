<template>
  <view class="pages_imgcategory">
    <view class="imgcategory_title">
      <view class="title_inner">
        <uni-segmented-control
          :current="current"
          :values="items.map((item) => item.title)"
          @clickItem="onClickItem"
          styleType="text"
          activeColor="#d4237a"
        >
        </uni-segmented-control>
      </view>
      <view class="iconfont iconsearch"> </view>
    </view>
    <scroll-view
      class="imgcategory_content"
      scroll-y
      enable-flex
      @scrolltolower="handleToLower"
    >
      <view class="list">
        <view class="item" v-for="item in vertical" :key="item.id">
          <image :src="item.img"></image>
        </view>
      </view>
      <!-- 没有更多 -->
      <nomore />
    </scroll-view>
  </view>
</template>

<script>
import nomore from "@/components/nomore";
// 引入分段器
import { uniSegmentedControl } from "@dcloudio/uni-ui";
export default {
  data() {
    return {
      items: [{ title: "最新" }, { title: "热门" }],
      current: 0,
      params: {
        limit: 15,
        skip: 0,
        order: "new",
      },
      id: "",
      vertical: [],
      // 判断接口是否还有数据
      isMore: true,
    };
  },
  methods: {
    onClickItem(e) {
      uni.setNavigationBarTitle({
        title: this.items[e.currentIndex].title,
      });
      if (this.current !== e.currentIndex) {
        this.current = e.currentIndex;
        // 切换分段器数组置空
        this.vertical = [];
        this.params.skip = 0;
        if (this.current == 0) this.params.order = "new";
        else this.params.order = "hot";
        this.getList(this.id);
      }
    },
    async getList(id) {
      let data = await this.request({
        url: `http://157.122.54.189:9088/image/v1/vertical/category/${id}/vertical`,
        data: this.params,
      });
      // 数据已经完全返回,isMore=false
      if (data.res.vertical.length == 0) {
        this.isMore = false;
        return;
      }
      this.vertical = [...this.vertical, ...data.res.vertical];
      console.log("data", this.vertical);
    },
    async handleToLower() {
      /**
       * 1.修改请求参数 skip+=limit;
       * 2.重新发送请求 getList()
       * 3.请求回来成功 hots 数据叠加
       */
      if (this.isMore) {
        this.params.skip += this.params.limit;
        this.getList(this.id);
      }

      console.log("触底");
    },
  },
  onLoad(options) {
    this.id = options.id;
    this.getList(this.id);
    console.log(options);
  },
  components: {
    nomore,
    uniSegmentedControl,
  },
};
</script>

<style lang="scss" scoped>
.pages_imgcategory {
  background: #fff;
  width: 100%;
  .imgcategory_title {
    position: relative;
    .title_inner {
      width: 60%;
      margin: 0 auto;
    }
    .iconsearch {
      position: absolute;
      top: 50%;
      right: 5%;
      transform: translateY(-50%);
    }
  }
  .imgcategory_content {
    height: calc(100vh - 36px);
    width: 100%;
    .list {
      display: flex;
      flex-wrap: wrap;
      width: 100%;
      .item {
        width: 33.3%;

        image {
          border: 1px solid #fff;
          box-sizing: border-box;
          width: 100%;
          height: 240rpx;
        }
      }
    }
  }
}
</style>