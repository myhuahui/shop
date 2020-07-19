<template>
  <div class="back-top">
      <img src="~assets/img/common/top.png" alt="" @click="ClickTop">
  </div>
</template>

<script>
export default {
    methods: {
        ClickTop(){
            this.$emit('ClickTop')
        }
    }
}
</script>

<style scoped>
.back-top{
    position: fixed;
    right: 8px;
    bottom: 55px;
}
.back-top img {
    width: 43px;
    height: 43px;
}
</style>