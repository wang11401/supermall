<template>
  <div v-if="Object.keys(detailInfo).length !== 0" class="goods-info">
    <div class="info-desc clear-fix">
      <div class="start"></div>
      <div class="desc">{{detailInfo.desc}}</div>
      <div class="end"></div>
    </div>
    <div class="info-key">{{detailInfo.detailImage[0].key}}</div>
    <div class="info-list">
      <img v-for="(item, index) in detailInfo.detailImage[0].list"
           :key="index" :src="item" @load="imgLoad" alt="">
    </div>
  </div>
</template>

<script>
  export default {
    name: "DetailGoodsInfo",
    props: {
      detailInfo: {
        type: Object,
        default() {
          return {}
        }
      }
    },
    data() {
      return {
        counter: 0,
        imagesLength: 0
      }
    },
    methods: {
      imgLoad() {
        if (++this.counter === this.imagesLength) {
          this.$emit('imageLoad')
        }
      }
    },
    watch: {
      detailInfo() {
        this.imagesLength = this.detailInfo.detailImage[0].list.length
      }
    }
  }
</script>

<style scoped>
  .goods-info {
    padding: 20px 0;
    border-bottom: 5px solid #f2f5f8;
  }
  .info-desc {
    margin: 2px 0;
  }
  .info-desc .start {
    height: 5px;
    width: 25vw;
    border-bottom: 2px solid #999;
    margin-left: 2px;
  }
  .info-desc .desc {
    width: 90vw;
    font-size: 8px;
    text-indent: 2em;
    text-align: left;
    margin: 2px auto;
    padding: 2px 0;
  }
  .info-desc .end {
    height: 1px;
    width: 25vw;
    float: right;
    border-bottom: 2px solid #999;
    margin-right: 2px;
  }
  .info-key {
    font-size: 13px;
    font-weight: bold;
  }
  .info-list {
    width: 100vw;
    margin: 2px auto;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
  }
  .info-list img {
    vertical-align: center;
    margin: 3px;
    width: calc(100% - 2%);
  }
</style>
