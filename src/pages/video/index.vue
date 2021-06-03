<template>
  <view class="video_tab">
    <view class="video_tab_title">
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
    <view class="video_tab_content">
      <view class="content">
        <view v-show="current <4"><video-main :current=current ></video-main> </view>
        <view v-show="current === 4"><video-category></video-category> </view>
      </view>
    </view>
  </view>
</template>

<script>
// 引入分段器
import { uniSegmentedControl } from "@dcloudio/uni-ui";
import videoCategory from "./video-category/index"
import videoMain from "./video-main/index"
export default {
  data() {
    return {
      items: [
        { title: "推荐" ,url:"http://157.122.54.189:9088/videoimg/v1/videowp/featured",params:{limit:30,skip:0,order:"hot"},url:"http://157.122.54.189:9088/videoimg/v1/videowp/featured",params:{limit:10,skip:0,order:"hot"} },
        { title: "娱乐" ,url:"http://157.122.54.189:9088/videoimg/v1/videowp/category/59b25abbe7bce76bc834198a",params:{limit:30,skip:0,order:"new"}},
        { title: "最新" ,url:"http://157.122.54.189:9088/videoimg/v1/videowp/videowp",params:{limit:30,skip:0,order:"new"}},
        { title: "热门" ,url:"http://157.122.54.189:9088/videoimg/v1/videowp/videowp",params:{limit:30,skip:0,order:"hot"}},
        { title: "分类" ,url:"http://157.122.54.189:9088/videoimg/v1/videowp/category",params:{}},
      ],
      current: 0,
    };
  },
  methods: {
    onClickItem(e) {
      uni.setNavigationBarTitle({
        title: this.items[e.currentIndex].title,
      });
      if (this.current !== e.currentIndex) {
        this.current = e.currentIndex;
      }
    },
  },
  components: {
    uniSegmentedControl,
    videoMain,
    videoCategory
  },
};
</script>

<style lang="scss" scoped>
.video_tab {
  background: #fff;
  width: 100%;
  .video_tab_title {
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
  .video_tab_content {
  }
}
</style>