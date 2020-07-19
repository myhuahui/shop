<template>
  <div id="home">
    <nav-bar class="nav-bar">
      <div slot="center">购物街</div>
    </nav-bar>
    <tab-control
      class="control3"
      :control="['流行', '新款', '精选']"
      @itemClick="itemClick"
      ref="tab2"
      v-show="isControl"
    ></tab-control>
    <scroll
      class="scroll"
      ref="scroll"
      :probeType="3"
      @scroll="isBackTop"
      :pullUpLoad="true"
      @pullingUp="loadMore"
    >
      <home-swiper-item
        :swiperData="banners"
        @swiperLoad="swiperLoad"
        ref="swiper"
      ></home-swiper-item>
      <home-recommend :recommends="recommends"></home-recommend>
      <home-popular></home-popular>
      <tab-control
        ref="tabcontrol"
        :control="['流行', '新款', '精选']"
        @itemClick="itemClick"
      ></tab-control>
      <goods-item :goods="goods[currentGoods].list"></goods-item>
    </scroll>
    <back-top v-show="isShowTop" @ClickTop="ClickTop"></back-top>
  </div>
</template>

<script>
import NavBar from "components/navbar/NavBar";
import HomeSwiperItem from "./childComps/HomeSwiperItem";
import HomeRecommend from "./childComps/HomeRecommend";
import HomePopular from "./childComps/HomePopular";
import TabControl from "components/tabControl/TabControl";
import GoodsItem from "components/goods/GoodsItem";
import Scroll from "components/scroll/Scroll";
import BackTop from "components/backTop/BackTop";

import { getHomeMultiData, getHomeGoods } from "network/home";
export default {
  components: {
    NavBar,
    HomeSwiperItem,
    HomeRecommend,
    HomePopular,
    TabControl,
    GoodsItem,
    Scroll,
    BackTop,
  },
  data() {
    return {
      banners: [],
      recommends: [],
      goods: {
        pop: { page: 1, list: [] },
        new: { page: 1, list: [] },
        sell: { page: 1, list: [] },
      },
      currentGoods: "pop",
      isShowTop: false,
      swiperOffsetTop: 0,
      isControl: false,
      savY: 0,
      homeImageLoad:null
    };
  },
  mounted() {
    const refresh = this.debounce(
      this.$refs.scroll && this.$refs.scroll.refresh,
      100
    );
    this.homeImageLoad = () => {
      refresh()
    }
    this.$bus.$on("ClickTop", this.homeImageLoad);
  },
  //页面重新进来时
  activated() {
    this.$refs.scroll.scrollTo(0, this.savY,0)
  },
  //页面离开时
  deactivated() {
    this.savY = this.$refs.scroll.getScrollY()
    this.$bus.$off('ClickTop',this.homeImageLoad)
  },
  created() {
    this.getHomeMultiData();
    this.getHomeGoods("pop");
    this.getHomeGoods("new");
    this.getHomeGoods("sell");
  },
  methods: {
    //轮播图加载完之后获取当前的offsetTop值
    swiperLoad() {
      this.swiperOffsetTop = this.$refs.tabcontrol.$el.offsetTop;
    },
    //上拉加载更多
    loadMore() {
      this.getHomeGoods(this.currentGoods);
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
    //点击切换分类
    itemClick(index) {
      switch (index) {
        case 0:
          this.currentGoods = "pop";
          break;
        case 1:
          this.currentGoods = "new";
          break;
        case 2:
          this.currentGoods = "sell";
      }
      this.$refs.tab2.currentIndex = index;
      this.$refs.tabcontrol.currentIndex = index;
    },
    //显示隐藏回到顶部图标
    isBackTop(position) {
      this.isShowTop = -position.y > 1000;
      this.isControl = -position.y > this.swiperOffsetTop;
    },
    //点击回到顶部
    ClickTop() {
      this.$refs.scroll.scrollTo(0, 0, 300);
    },
    getHomeMultiData() {
      getHomeMultiData().then((res) => {
        //获取轮播图数据
        this.banners = res.data.banner.list;
        //获取推荐列表数据
        this.recommends = res.data.recommend.list;
      });
    },
    getHomeGoods(type) {
      const page = this.goods[type].page + 1;
      getHomeGoods(type, page).then((res) => {
        this.goods[type].list.push(...res.data.list);
        this.goods[type].page += 1;

        this.$refs.scroll.scroll.finishPullUp();
      });
    },
  },
};
</script>

<style scoped>
#home {
  height: 100vh;
  position: relative;
  padding-top: 44px;
}
.scroll {
  overflow: hidden;
  position: absolute;
  top: 44px;
  bottom: 49px;
  left: 0px;
  right: 0px;
}
.nav-bar {
  background-color: #ff8198;
  color: #fff;

  position: fixed;
  left: 0;
  top: 0;
  right: 0;
  z-index: 9;
}
.control3 {
  position: relative;

  z-index: 9;
  background-color: #fff;
}
</style>
