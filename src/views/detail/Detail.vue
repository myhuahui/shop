<template>
  <div class="detail">
    <detail-nav-bar
      class="nav"
      @itemClick="itemClick"
      ref="navB"
    ></detail-nav-bar>
    <scroll class="content" ref="scroll" :probeType="3" @scroll="scrollClick">
      <detail-swiper :topimage="topImages"></detail-swiper>
      <detail-base-info :goods="goods"></detail-base-info>
      <detail-shop-info :shop="shop"></detail-shop-info>
      <detail-goods-info
        :detailInfo="detailInfo"
        @imageLoad="imageLoad"
      ></detail-goods-info>
      <detail-param-info :paramInfo="paramInfo" ref="param"></detail-param-info>
      <detail-comment-info
        :commentInfo="commentInfo"
        ref="comment"
      ></detail-comment-info>
      <goods-item :goods="recommendList" ref="recommend"></goods-item>
    </scroll>
    <back-top v-show="isShowBackTop" @ClickTop="ClickTop"></back-top>
    <detail-bottom-bar></detail-bottom-bar>
  </div>
</template>

<script>
import {
  getDetail,
  Goods,
  Shop,
  GoodsParam,
  getRecomend,
} from "network/detail";
import DetailNavBar from "./childComps/DetailNavBar";
import DetailSwiper from "./childComps/DetailSwiper";
import DetailBaseInfo from "./childComps/DetailBaseInfo";
import DetailShopInfo from "./childComps/DetailShopInfo";
import Scroll from "components/scroll/Scroll";
import DetailGoodsInfo from "./childComps/DetailGoodsInfo";
import DetailParamInfo from "./childComps/DetailParamInfo";
import DetailCommentInfo from "./childComps/DetailCommentInfo";
import GoodsItem from "components/goods/GoodsItem";
import BackTop from "components/backTop/BackTop";
import DetailBottomBar from "./childComps/DetailBottomBar"
export default {
  name: "Detail",
  data() {
    return {
      iid: null,
      topImages: [],
      goods: {},
      shop: {},
      detailInfo: {},
      paramInfo: {},
      commentInfo: {},
      recommendList: [],
      isShowBackTop: false,
      currentOffsetTopY: [],
      getCurrentOffsetTopY: null,
      detailImageLoad: null,
      currentIndex: 0,
    };
  },
  components: {
    DetailNavBar,
    DetailSwiper,
    DetailBaseInfo,
    DetailShopInfo,
    Scroll,
    DetailGoodsInfo,
    DetailParamInfo,
    DetailCommentInfo,
    GoodsItem,
    BackTop,
    DetailBottomBar
  },
  created() {
    this.iid = this.$route.params.iid;
    getDetail(this.iid).then((res) => {
      this.topImages = res.result.itemInfo.topImages;
      //商品详细信息
      this.goods = new Goods(
        res.result.itemInfo,
        res.result.columns,
        res.result.shopInfo.services
      );
      //店铺详细信息
      this.shop = new Shop(res.result.shopInfo);
      //商品详细页
      this.detailInfo = res.result.detailInfo;
      //获取参数信息
      this.paramInfo = new GoodsParam(
        res.result.itemParams.info,
        res.result.itemParams.rule
      );
      //评论信息
      if (res.result.rate.list.length > 0) {
        this.commentInfo = res.result.rate.list[0];
      }
      getRecomend().then((res) => {
        this.recommendList = res.data.list;
      });
      this.getCurrentOffsetTopY = this.debounce(() => {
        this.currentOffsetTopY = [];
        this.currentOffsetTopY.push(0);
        this.currentOffsetTopY.push(this.$refs.param.$el.offsetTop - 44);
        this.currentOffsetTopY.push(this.$refs.comment.$el.offsetTop - 44);
        this.currentOffsetTopY.push(this.$refs.recommend.$el.offsetTop - 44);
      }, 100);
    });
  },
  mounted() {
    const refresh = this.debounce(
      this.$refs.scroll && this.$refs.scroll.refresh
    );
    this.detailImageLoad = () => {
      refresh();
    };
    this.$bus.$on("imgLoad", this.detailImageLoad);
  },
  destroyed() {
    this.$bus.$off("imgLoad", this.detailImageLoad);
  },
  methods: {
    imageLoad() {
      this.$refs.scroll.refresh();
      this.getCurrentOffsetTopY();
    },
    scrollClick(position) {
      let positionY = -position.y;
      let length = this.currentOffsetTopY.length;
      for (let i = 0; i < length; i++) {
        if (
          this.currentIndex !== i &&
          ((i < length - 1 &&
            positionY >= this.currentOffsetTopY[i] &&
            positionY <= this.currentOffsetTopY[i + 1]) ||
            (i === length - 1 && positionY >= this.currentOffsetTopY[i]))
        ) {
          this.currentIndex = i;
          this.$refs.navB.currentIndex = this.currentIndex;
        }
      }

      this.isShowBackTop = -position.y > 1000;
    },
    ClickTop() {
      this.$refs.scroll.scrollTo(0, 0, 200);
    },
    itemClick(index) {
      this.$refs.scroll.scrollTo(0, -this.currentOffsetTopY[index], 200);
    },
    //防抖函数
    debounce(func, delay) {
      let timer = null;
      return function(...args) {
        if (timer) clearTimeout(timer);
        timer = setTimeout(() => {
          func.apply(this, args);
        }, delay);
      };
    },
  },
};
</script>

<style scoped>
.detail {
  height: 100vh;
  position: relative;
  background-color: #fff;
  z-index: 5;
}
.content {
  height: calc(100% - 93px);
  background-color: #fff;
}
.nav {
  position: relative;
  z-index: 9;
  background-color: #fff;
}
</style>
