<template>
  <div class="control">
      <div class="control-item" v-for="(item,index) in control" :key="index" :class="{active: index === currentIndex}" @click="itemClick(index)">
          <span>{{item}}</span>
      </div>
  </div>
</template>

<script>
export default {
    props: {
        control: {
        type: Array,
        default(){
            return []
        }
        }
    },
    data(){
        return {
            currentIndex: 0
        }
    },
    methods: {
        itemClick(index){
            this.currentIndex = index
            this.$emit('itemClick',index)
        }
    }
}
</script>

<style scoped>
.control{
    height: 40px;
    display: flex;
    text-align: center;
    padding: 10px 0px ;
    font-size: 15px;
}
.control-item{
    flex: 1;
    
}
.control-item span {
    padding-bottom: 5px;
}
.active{
    color: #ff8198;
}
.active span {
    border-bottom: 3px solid #ff5777;
}
</style>>