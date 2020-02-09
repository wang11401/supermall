<template>
  <div id="detail">
    <detail-nav-bar class="detail-nav" @titleClick="titleClick" ref="detailNav"/>
    <scroll class="content" ref="scroll"
            @scroll="contentScroll"
            :probe-type="3">
      <detail-swiper class="detail-swiper" :top-images="topImages"/>
      <detail-base-info :goods="goods"/>
      <detail-shop-info :shop="shop"/>
      <detail-goods-info :detail-info="detailInfo" @imageLoad="imageLoad"/>
      <detail-param-info ref="param" :paramInfo="paramInfo"/>
      <detail-comment-info ref="comment" :commentInfo="commentInfo"/>
      <goods-list ref="recommends" :goods="recommends"/>
    </scroll>
  </div>
</template>

<script>
  import DetailNavBar from "views/detail/childComps/DetailNavBar";
  import DetailSwiper from "views/detail/childComps/DetailSwiper";
  import DetailBaseInfo from "views/detail/childComps/DetailBaseInfo";
  import DetailShopInfo from "views/detail/childComps/DetailShopInfo";
  import DetailGoodsInfo from "views/detail/childComps/DetailGoodsInfo";
  import DetailParamInfo from "views/detail/childComps/DetailParamInfo";
  import DetailCommentInfo from "views/detail/childComps/DetailCommentInfo";

  import Scroll from "components/common/scroll/Scroll";

  import {getDetail, Goods, Shop, GoodsParam, getRecommend} from "network/detail";

  import GoodsList from "components/content/goods/GoodsList";

  import {itemListenerMixin} from "common/mixin";

  import {debounce} from "common/utils";

  export default {
    name: "Detail",
    mixins: [itemListenerMixin],
    data() {
      return {
        iid: null,
        topImages: [],
        goods: {},
        shop: {},
        detailInfo: {},
        paramInfo: {},
        commentInfo: {},
        recommends: [],
        themeTopYs: [],
        getThemeTopY: null,
        currentIndex: 0
      }
    },
    created() {
      //保存传入的id
      this.iid = this.$route.params.id

      getDetail(this.iid).then(res => {
        const data = res.result
        // console.log(res);
        //获取顶部轮播图片数据
        this.topImages = data.itemInfo.topImages
        //获取商品信息
        this.goods = new Goods(data.itemInfo, data.columns, data.shopInfo.services)
        //获取店铺信息
        this.shop = new Shop(data.shopInfo)
        //获取商品详细信息
        this.detailInfo = data.detailInfo
        //获取参数信息
        this.paramInfo = new GoodsParam(data.itemParams.info, data.itemParams.rule)
        //获取评论信息
        if(data.rate.cRate !== 0) {
          this.commentInfo = data.rate.list[0]
        }
      })
      //请求推荐数据
      getRecommend().then(res => {
        // console.log(res);
        this.recommends = res.data.list
      })
      this.getThemeTopY = debounce(() => {
        this.themeTopYs = []
        this.themeTopYs.push(0)
        this.themeTopYs.push(this.$refs.param.$el.offsetTop)
        this.themeTopYs.push(this.$refs.comment.$el.offsetTop)
        this.themeTopYs.push(this.$refs.recommends.$el.offsetTop)
        console.log(this.themeTopYs);
      })
    },
    mounted() {
    },
    updated() {
    },
    destroy() {
      this.$bus.$off('itemImageLoad', this.itemImgListener)
    },
    components: {
      DetailNavBar,
      DetailSwiper,
      DetailBaseInfo,
      DetailShopInfo,
      DetailGoodsInfo,
      DetailParamInfo,
      DetailCommentInfo,
      Scroll,
      GoodsList
    },
    methods: {
      imageLoad() {
        this.$refs.scroll.refresh()
        this.getThemeTopY()
      },
      titleClick(index) {
        // console.log(index);
        this.$refs.scroll.scrollTo(0, -this.themeTopYs[index]+44, 500)
      },
      contentScroll(position) {
        // console.log(position);
        //保存滚动的y值
        const positionY = -position.y
        //与详情页的offsetTop对比
        let length = this.themeTopYs.length
        for(let i = 0; i < length; i++) {
          // console.log(i);
          // if(positionY > this.themeTopYs[i]
          //   && positionY < this.themeTopYs[i+1]) {
          //   console.log(i);
          // }
          if(this.currentIndex !== i && ((i < length - 1
            &&  positionY >= this.themeTopYs[i]
            &&  positionY < this.themeTopYs[i+1])
            || (i === length -1 &&  positionY >= this.themeTopYs[i]))) {
            this.currentIndex = i
            this.$refs.detailNav.currentIndex = this.currentIndex
          }
        }
      }
    }
  }
</script>

<style scoped>
  #detail {
    position: relative;
    z-index: 9;
    background-color: #fff;
    height: 100vh;
  }
  .content {
    /*margin-top: 44px;*/
    height: calc(100% - 44px)
  }
  /*.content .detail-swiper{*/
  /*  margin-top: 44px;*/
  /*}*/
  .detail-nav {
    position: relative;
    z-index: 9;
    background-color: #fff;
    /*margin-bottom: 44px;*/
  }
</style>
