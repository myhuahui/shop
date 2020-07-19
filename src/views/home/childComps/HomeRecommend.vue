<template>
  <div class="recommend">
      <div class="rec-item" v-for="item in recommends" :key="item.index">
          <a :href="item.link">
              <img :src="item.image" alt="">
              {{item.title}}
          </a>
      </div>
  </div>
</template>

<script>
export default {
    props: {
        recommends: {
            type: Array,
            default(){
                return []
            }
        }
    }
}
</script>

<style scoped>
.recommend {
    display: flex;
    padding: 15px 0 30px;
    text-align: center;
    font-size: 13px;
    border-bottom: 8px solid #eee;
}
.rec-item{
    flex: 1;
}
.recommend img {
    width: 78px;
    height: 78px;
    margin-bottom: 10px;
}
</style>