<template>
    <!-- <p>{{ this.lspaces }}</p> -->
    <div class="lesson-comp">
       <p >Name: {{ this.data.name }}</p>
       <p>Price: {{ this.data.price }}</p> 
       <div v-if="this.pageNo===1" v-for="items in this.lspaces" key="items._id">
          <p v-if="items._id===this.data._id">Spaces: {{ items.spaces}}</p>
       </div>
       <div v-if="this.pageNo===2" v-for="items in this.cspaces" key="items._id">
          <p v-if="items._id===this.data._id">spaces: {{ items.spaces}}</p>
       </div>
      
       <button @click="handleAdd" v-if="this.pageNo===1"  id="add-btn" >Add to Cart</button>
       <button @click="handleRemove" v-if="this.pageNo===2" id="remove-btn">Remove</button>
    </div>
</template>

<script>
    export default {
        name:'LessonComp',
        props:['data','addId','removeId','pageNo','lspaces','cspaces'],
        methods:{
            handleAdd:function(){
                
                this.$emit('addId',this.data._id)
                this.$forceUpdate()
            },
            handleRemove(){
                this.$emit('removeId',this.data._id)
                this.$forceUpdate()
            }
        },
       
    }
</script>

<style lang="scss" scoped>
 
</style>