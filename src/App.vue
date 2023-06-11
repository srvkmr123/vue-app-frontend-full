
<template>
  <div class="header">
    <h2 v-if="page==0">Home </h2>
    <h2 v-if="page==1">Lessons </h2>
    <h2 v-if="page==2">Checkout </h2>
    <div class="right">
      <button @click="page=0" id="btn0">Home</button>
      <button @click="page=1" id="btn1">Lesson</button>
      <button @click="page=2" id="btn2">Checkout</button>
      <button class="cart-btn" @click="myCart=!myCart">cart</button>
      <h1 v-if="this.myCart===true">{{ cart.length }}</h1>
    </div>  
  </div>
  <h1 v-if="page===0" style="text-align: center;">
    <h1>My Cart</h1>
    <button class="cart-btn-2" @click="myCart=!myCart"><img src="../src/assets/download.jpg" alt=""></button>
    <h1 v-if="this.myCart===true" style="color: #ff1a1a;"><small style="font-size: 0.5em;color: black;">Items in cart: </small>{{ cart.length }}</h1>
  </h1>
  <Lesson v-if="page===1" msg="List of all Lessons!!" :lessons="lessons" @aId="addToCart($event)" :pageNo="this.page" :lspaces="lspaces"/>
  <Checkout v-if="page===2" msg="" :lessons="cart" @rId="removeFromCart($event)" :pageNo="this.page" :cspaces="cspaces"/>
</template>

<script>
  
  import Lesson from './components/Lesson.vue'
  import Checkout from './components/Checkout.vue'
  import axios from 'axios'
  import { isProxy, toRaw } from 'vue';
  
  
  export default{
  name:"App",
  components:{Lesson,Checkout},
  data(){
    return{
      page:0,
      lessons:[],
      cart:[],
      lspaces:[],
      cspaces:[],
     
      myCart:false,
    }
  },
  methods:{
     addToCart(id){
      
      toRaw(this.lspaces).forEach(item=>{
         if(item._id===id)
           {
            if(item.spaces===0)
            return
            item.spaces--;
           
            //console.log(item.spaces)
          }
         })
         
         toRaw(this.cspaces).forEach(item=>{
         if(item._id===id && item.spaces<item.max )
           {
            item.spaces++;
            //console.log(item.spaces)
          }
         })
        //console.log(this.lspaces) 
     if(this.cart.find(item=>item._id===id))
        return
     this.cart.push(toRaw(this.lessons).find(item=>item._id===id))
     
     
   },
   removeFromCart(id){
    
    toRaw(this.cspaces).forEach(item=>{
      //.log("remove")
         if(item._id===id )
           {
            if(item.spaces===1)
             {
              item.spaces=0
              this.cart=this.cart.filter(item=>item._id!==id)
             }
            else 
             item.spaces--;
            //console.log(item.spaces)
          }
          
         })
    toRaw(this.lspaces).forEach(item=>{
         if(item._id===id && item.spaces<item.max)
           {
            item.spaces++;
            //console.log(item.spaces)
          }
         })    
    // else
    //  this.cart=this.cart.filter(item=>item._id!==id)
     
   }
  },
  mounted(){
    axios('http://127.0.0.1:8080/api/lessons')
    .then(res=>{
      this.lessons=res.data
      toRaw(this.lessons).forEach(ele=>{
        this.lspaces.push({_id:ele._id,spaces:ele.spaces,max:ele.spaces})
        this.cspaces.push({_id:ele._id,spaces:0,max:ele.spaces})
       
      })
      //console.log(this.lspaces,this.cspaces)
    })
    .catch(err=>console.log(err))
  }
  }
</script>