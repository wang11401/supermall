<template>
  <nav-bar>
    <div slot="left">
      <div class="backBtn" @click="backClick"></div>
    </div>
    <div slot="center" class="title">
      <div v-for="(item, index) in titles" class="titleItem"
            @click="itemClick(index)"
            :class="{isActive:currentIndex == index}">
        {{item}}</div>
    </div>
  </nav-bar>
</template>

<script>
  import NavBar from "components/common/navbar/NavBar";

  export default {
    name: "DetailNavBar",
    components: {
      NavBar
    },
    data() {
      return {
        titles: ['商品','参数','评论','推荐'],
        currentIndex: 0
      }
    },
    methods: {
      itemClick(index) {
        this.currentIndex = index;
        this.$emit('titleClick', index)
      },
      backClick() {
        this.$router.back()
      }
    }
  }
</script>

<style scoped>
  .title {
    display: flex;
    font-size: 13px;
    text-align: center;
  }
  .titleItem {
    flex: 1;
  }
  .isActive {
    color: var(--color-high-text);
  }
  .backBtn {
    height: 0;
    width: 0;
    position: relative;
    left: 15px;
    top: 12px;
    border: 10px solid;
    border-color: transparent #000 transparent transparent;
  }
  .backBtn::after {
    content: '';
    position: absolute;
    border: 10px solid;
    border-color: transparent #fff transparent transparent;
    left: -9px;
    top: -10px;
  }
</style>
