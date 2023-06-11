<template>
     <h2 style="text-align: center;color: black;">Welcome to checkout page!! </h2>
    <div v-if="this.lessons.length!==0" class="lesson-container">
        <div  v-for="item in this.lessons" :key="item._id" class="lessons">
           <LessonComp :data="item" @removeId="removeId($event)" :pageNo="this.pageNo" :cspaces="this.cspaces"/>
        </div>
    </div>
    <h1 v-else style="text-align: center; color: #ff1a1a;">Cart is Empty! Add Lessons to Cart</h1>
    
</template>

<script>
import LessonComp from './LessonComp.vue'
    export default {
        name:"Checkout",
        props:['msg','lessons','pageNo','cspaces'],
        components:{LessonComp},
        emits:['rId'],
        methods:{
            removeId(id){
                this.$emit('rId',id)
            }
        },
        
    }
</script>

<style lang="scss" scoped>

</style>