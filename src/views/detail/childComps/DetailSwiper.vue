<template>
  <swiper class="swiper-item">
      <swiper-item v-for="item in topimage" :key="item.index">
          <img :src="item" alt="">
      </swiper-item>
  </swiper>
</template>

<script>
import {Swiper,SwiperItem} from "components/swiper"
export default {
    props: {
        topimage: {
            type: Array,
            default(){
                return []
            }
        }
    },
    components: {
        Swiper,
        SwiperItem
    }
}
</script>

<style scoped>
.swiper-item{
    height: 300px;
    overflow: hidden;
}
</style>