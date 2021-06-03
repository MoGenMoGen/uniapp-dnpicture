<template>
  <scroll-view
    scroll-y="true"
    :scroll-top="scrolltop"
    @scrolltolower="handleToLower"
    @scroll="handlescroll"
    class="video_main"
  >
    <view class="list">
      <view class="item" v-for="(item,index) in videowp" :key="item.id" >
      <!-- @click="gotovideo(item)" -->
          <go-img :list="videowp" :index="index" :isvideo=true>
        <image :src="item.img"></image>
          </go-img>
      </view>
    </view>
    <view class="backtop" v-show="canbacktop" @click.stop="returnTop"
      >回到顶部</view
    >
    <nomore />
  </scroll-view>
</template>

<script>
import goImg from "@/components/goImg"
import nomore from "@/components/nomore";

export default {
  props: {
    current: {
      type: Number,
      default() {
        return 0;
      },
    },
  },
  data() {
    return {
      items: [
        {
          url: "http://157.122.54.189:9088/videoimg/v1/videowp/featured",
          params: { limit: 30, skip: 0, order: "hot" },
          isMore: true,
        },
        {
          url:
            "http://157.122.54.189:9088/videoimg/v1/videowp/category/59b25abbe7bce76bc834198a",
          params: { limit: 30, skip: 0, order: "new" },
          isMore: true,
        },
        {
          url: "http://157.122.54.189:9088/videoimg/v1/videowp/videowp",
          params: { limit: 30, skip: 0, order: "new" },
          isMore: true,
        },
        {
          url: "http://157.122.54.189:9088/videoimg/v1/videowp/videowp",
          params: { limit: 30, skip: 0, order: "hot" },
          isMore: true,
        },
      ],
      videowp: [],
      scrolltop: 0,
      oldscrolltop: 0,
      canbacktop: false,
    };
  },
  watch: {
    current() {
      if (this.current < 4) {
        console.log("current发生变化", this.current);
        this.videowp = [];
        this.scrolltop = 0;
        this.canbacktop = false;
        this.getList();
      }
    },
  },
  components: {
    nomore,
    goImg
  },
  mounted() {
    console.log(this.current);
    this.getList();
  },
  methods: {
    async getList() {
      let data = await this.request({
        url: this.items[this.current].url,
        data: this.items[this.current].params,
      });
      if (data.res.videowp.length != 0)
        this.videowp = [...this.videowp, ...data.res.videowp];
      else {
        this.items[this.current].isMore = false;
        return false;
      }
    },
    handleToLower() {
      console.log("触底");
      if (this.items[this.current].isMore) {
        this.items[this.current].params.skip += this.items[
          this.current
        ].params.limit;
        this.getList();
      }
    },
    handlescroll(e) {
      this.oldscrolltop = e.detail.scrollTop;
      if (e.detail.scrollTop > 100) {
        this.canbacktop = true;
      } else {
        this.canbacktop = false;
      }
    },
    returnTop() {
      //视图会发生重新渲染
      this.scrolltop = this.oldscrolltop;
      //当视图渲染结束 重新设置为0
      this.$nextTick(() => {
        this.scrolltop = 0;
      });
    },
    // 跳转到视频播放
    gotovideo(item){
        // 将图片地址放到globaldata
        getApp().globalData.video=item;
        // 跳转页面
        uni.navigateTo({
             url: '/pages/videoplay/index'
        });
    }
  },
};
</script>

<style lang="scss" scoped>
.video_main {
  height: calc(100vh - 36px);
  width: 100%;
  .list {
    width: 100%;
    display: flex;
    flex-wrap: wrap;
    .item {
      width: 33.3%;
      border: 2px solid #fff;
      image {
        width: 100%;
        height: 240rpx;
      }
    }
  }
  .backtop {
    font-size: 36rpx;
    width: 100rpx;
    height: 100rpx;
    background: #fff;
    color: #000;
    border-radius: 50%;
    position: fixed;
    bottom: 100rpx;
    right: 30rpx;
    text-align: center;
    padding: 5rpx 0;
  }
}
</style>