<template>
  <scroll-view
    scroll-y
    class="home_category"
    v-if="category.length > 0"
  >
    <view class="list">
      <navigator class="item" v-for="item in category" :key="item.id" :url="`/pages/imgCategory/index?id=${item.id}`">
        <image :src="item.cover"></image>
        <text class="item_name">{{item.name}}</text>
      </navigator>
    </view>
  </scroll-view>
 
</template>

<script>
export default {
  data() {
    return {
      category: [],
    };
  },
  mounted() {
    this.getList();
  },
  methods: {
    async getList() {
      let data = await this.request({
        url: "http://157.122.54.189:9088/image/v1/vertical/category",
      });
      this.category = data.res.category;
      console.log(this.category);
    },
  },
};
</script>

<style lang="scss" scoped>
.home_category {
  height: calc(100vh - 36px);
  width: 100%;
  .list {
    display: flex;
    flex-wrap: wrap;
    width: 100%;
    .item {
      position: relative;
      width: 33.3%;
      border: 1px solid #fff;

      image {
        height: 240rpx;
      }
      .item_name{
        font-size: 36rpx;
        color:#fff;
        position: absolute;
        bottom:1px;
        left:20rpx;
      }
    }
  }
}
</style>