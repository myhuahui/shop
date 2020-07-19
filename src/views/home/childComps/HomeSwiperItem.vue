<template>
  <swiper>
      <swiper-item v-for="item in swiperData" :key="item.index">
          <a :href="item.link">
              <img :src="item.image" alt="" @load="swiperLoad">
          </a>
      </swiper-item>
  </swiper>
</template>

<script>
import {Swiper, SwiperItem} from "components/swiper"
export default {
    props: {
        swiperData: {
            type: Array,
            default(){
                return []
            }
        }
    },
    components: {
        Swiper,
        SwiperItem
    },
    data(){
        return {
            isLoad:false
        }
    },
    methods: {
        swiperLoad(){
            if(!this.isLoad){
                this.$emit('swiperLoad')
                this.isLoad = true
            }
        }
    }
}
</script>

<style scoped>

</style>