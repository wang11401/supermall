<template>
  <tab-bar>
    <tab-bar-item path="/home">
      <img slot="item-icon" src="~assets/img/tabbar/1.svg" alt="">
      <img slot="item-icon-active" src="~assets/img/tabbar/icon-home.svg" alt="">
      <div slot="item-text">首页</div>
    </tab-bar-item>

    <tab-bar-item path="/category">
      <img slot="item-icon" src="~assets/img/tabbar/fenlei.svg" alt="">
      <img slot="item-icon-active" src="~assets/img/tabbar/leimupinleifenleileibie2.svg" alt="">
      <div slot="item-text">分类</div>
    </tab-bar-item>

    <tab-bar-item path="/cart">
      <img slot="item-icon" src="~assets/img/tabbar/shangcheng.svg" alt="">
      <img slot="item-icon-active" src="~assets/img/tabbar/gouwucheman.svg" alt="">
      <div slot="item-text">购物车</div>
    </tab-bar-item>

    <tab-bar-item path="/profile">
      <img slot="item-icon" src="~assets/img/tabbar/weibiaoti-.svg" alt="">
      <img slot="item-icon-active" src="~assets/img/tabbar/wodedangxuan.svg" alt="">
      <div slot="item-text">我的</div>
    </tab-bar-item>
  </tab-bar>
</template>

<script>
  import TabBar from "components/common/tabbar/TabBar";
  import TabBarItem from "components/common/tabbar/TabBarItem";
  export default {
    name: "MainTabBar",
    components: {
      TabBar,
      TabBarItem
    }
  }
</script>

<style scoped>

</style>
