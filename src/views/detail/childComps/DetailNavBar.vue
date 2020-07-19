<template>
  <div>
      <nav-bar>   
          <div class="left" slot="left"><img src="~assets/img/common/back.svg" alt="" @click="imgClick"></div>
              <div class="nav-bar" slot="center">
                  <div class="di" v-for="(item,index) in titles" :key="index" :class="{active: index === currentIndex}" @click="itemClick(index)">{{item}}</div>
              </div>
      </nav-bar>
  </div>
</template>

<script>
import NavBar from "components/navbar/NavBar"
export default {
    components: {
        NavBar
    },
    data(){
        return {
            titles: ['商品','参数','评论','推荐'],
            currentIndex: 0
        }
    },
    methods: {
        itemClick(index){
            this.currentIndex = index
            this.$emit('itemClick',index)
        },
        imgClick(){
            this.$router.back()
        }
    }
}
</script>

<style scoped>
.nav-bar{
   display: flex;
   font-size: 13px;
}
.nav-bar  .di{
    flex: 1;
}
.active{
    color: #ff5777;
}
.left img {
    margin-top: 12px;
}
</style>