<template>
  <div class="goods">
      <goods-item-list v-for="item in goods" :key="item.index" :goodsList="item"></goods-item-list>
  </div>
</template>

<script>
import GoodsItemList from "./GoodsItemList"
export default {
    props: {
        goods: {
            type: Array,
            default(){
                return []
            }
        }
    },
    components: {
        GoodsItemList
    }
}
</script>

<style scoped>
.goods {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
}
</style>