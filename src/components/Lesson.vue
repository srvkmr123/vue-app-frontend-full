<template>
    <div style="text-align: center;">
       <h2 style="color: black;">Welcome to Lessons!</h2>
    </div>
    <div  class="lesson-container">
        <div v-for="item in this.lessons" :key="item._id"  class="lessons">
           <LessonComp :data="item" @addId="addId($event)" :pageNo="this.pageNo" :lspaces="this.lspaces"/> 
        </div>
    </div>
</template>

<script>
    import LessonComp from './LessonComp.vue'
    import { toRaw } from 'vue'
    export default {
     name:"Lesson",
     props:['msg','lessons','pageNo','lspaces'],
     emits:['aId'],
     components:{LessonComp},
     methods:{
        addId:function(id){
           this.$emit('aId',id)
        }
     },
     data(){
        return{
          space:0,
        }
     }
    }
</script>

<style lang="scss" scoped>

</style>