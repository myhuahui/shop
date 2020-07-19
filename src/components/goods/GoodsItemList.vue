<template>
  <div class="goods-item">
    <img :src="showImage" alt="" @load="imgLoad" @click="imgClick" />
    <div class="goods-info">
      <p>{{ goodsList.title }}</p>
      <span class="price">{{ goodsList.price }}</span>
      <span class="cfav">{{ goodsList.cfav }}</span>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    goodsList: {
      type: Object,
      default() {
        return {};
      },
    },
  },
  methods: {
    imgLoad() {
      this.$bus.$emit("imgLoad");
    },
    imgClick() {
      this.$router.push("/detail/" + this.goodsList.iid);
    },
  },
  computed: {
    showImage() {
      return this.goodsList.image || this.goodsList.show.img;
    },
  },
};
</script>

<style scoped>
.goods-item {
  padding-bottom: 40px;
  position: relative;

  width: 48%;
}
.goods-item img {
  width: 100%;
  border-radius: 5px;
  flex: 1;
}
.goods-info {
  font-size: 12px;
  position: absolute;
  bottom: 5px;
  left: 0;
  right: 0;
  overflow: hidden;
  text-align: center;
}
.goods-info p {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  margin-bottom: 3px;
}
.goods-info .price {
  color: rgb(255, 87, 119);
  margin-right: 20px;
}
.goods-info .cfav {
  position: relative;
}
.goods-info .cfav::before {
  content: "";
  position: absolute;
  left: -15px;
  top: -1px;
  width: 14px;
  height: 14px;
  background: url(~assets/img/common/collect.svg) 0 0/14px 14px;
}
</style>
