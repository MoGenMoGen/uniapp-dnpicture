<template>
  <view class="home_tab">
    <view class="home_tab_title">
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
     <view class="iconfont iconsearch">
     </view>
    </view>
    <view class="home_tab_content">
      <view class="content">
      <view v-show="current === 0">
        <home-recommend />
      </view>
      <view v-show="current === 1">
        <home-category />
      </view>
      <view v-show="current === 2">
        <home-new />
      </view>
      <view v-show="current === 3">
        <home-album />
      </view>
    </view>
    </view>
  </view>
</template>

<script>
import homeAlbum from "./ChildComponents/home-album";
import homeCategory from "./ChildComponents/home-category";
import homeNew from "./ChildComponents/home-new";
import homeRecommend from "./ChildComponents/home-recommend";
// 引入分段器
import { uniSegmentedControl } from "@dcloudio/uni-ui";
export default {
  components: {
    homeAlbum,
    homeCategory,
    homeNew,
    homeRecommend,
    uniSegmentedControl,
  },
  data() {
    return {
      items: [
        { title: "推荐" },
        { title: "分类" },
        { title: "最新" },
        { title: "专辑" },
      ],
      current: 1,
    };
  },
  methods: {
    onClickItem(e) {
      if (this.current !== e.currentIndex) {
        this.current = e.currentIndex;
      }
    },
  },
  onLoad(){
console.log("onload");
  },
  async onShow(){
  }
};
</script>

<style lang="scss">
.home_tab{
  width: 100%;
  .home_tab_title{
    position: relative;
    .title_inner{
      width: 60%;
      margin:0 auto;
    }
    .iconsearch{
      position: absolute;
      top:50%;
      right:5%;
      transform:translateY(-50%)
    }
  }
  .home_tab_content{

  }
}
</style>

