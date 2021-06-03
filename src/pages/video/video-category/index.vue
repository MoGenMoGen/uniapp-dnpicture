<template>
  <view>5</view>
</template>

<script>
export default {
  data() {
    return {
      url: "http://157.122.54.189:9088/videoimg/v1/videowp/category",
    };
  },
  mounted() {
      console.log("222");
    this.getList();
  },
  methods: {
    async getList() {
        console.log(1111);
      let data = await this.request({
        url: this.url,
        data:{}
      });
      console.log(data);
    },
  },
};
</script>

<style lang="scss" scoped>
</style>