<template>
    <link href='https://unpkg.com/boxicons@2.1.4/css/boxicons.min.css' rel='stylesheet'>
  <header>
    <h1>{{sitename}}</h1>
    <div style="align-self: flex-end; margin-right: 2rem;" v-if="showProducts">
        <button style="font-size: 1.15rem;" v-if="canRemoveFromCart" @click="cycle">
            {{cartItemCount}}
            <i class="bx bx-cart"></i>
            Checkout
        </button>
        <button v-else style="font-size: 1.15rem;" disabled="disabled">
            {{cartItemCount}}
            <i class="bx bx-cart"></i>
        </button>
    </div>
    <div v-else>
      <button style="font-size: 1.15rem;" @click="cycle">
          <i class='bx bx-left-arrow-alt'></i>
          Go back
      </button>
    </div>
    <!-- <button @click="cycle" >{{currentViewStr}}</button> -->
  </header>
  <main>
    <component :is="currentView" :cart-items="cart" :product-items="products"  
    @add-cart="handleAddToCart"  @remove-cart="handleRemoveFromCart" @order-submitted="handleOrderSubmitted"/>
    <!-- <LessonComponent @add-cart-event="HandleCartEvents"></LessonComponent> -->
  </main>
</template>

<script>
import CartComponent from './components/Cart.vue'
import LessonComponent from './components/Lesson.vue'


export default {
  name: 'App',
  computed:{
    cartItemCount(){
        return this.cart.length || "Empty";
    },
    canRemoveFromCart(){
        return this.cart.length > 0
    },
  },
  data(){
      // console.log(LessonComponent.data()["cart"]);
      return {
          sitename:"After School Activities Store",
          currentView: LessonComponent,
          currentViewStr: "Cart",
          showProducts:true,
          cart:[],
          products:[
            {
              id:1,
              subject:"Art And Craft",
              location:"London",
              price:22,
              spaces:5,
              image:"/assets/art_and_craft.jpg"
          },
          {
              id:2,
              subject:"Sport",
              location:"Oxford",
              price:19,
              spaces:5,
              image:"/assets/sport.jpg"
          },
          {
              id:3,
              subject:"Music",
              location:"London",
              price:20,
              spaces:5,
              image:"/assets/music.jpg"
          },
          {
              id:4,
              subject:"Theater",
              location:"York",
              price:17,
              spaces:5,
              image:"/assets/theater.jpg"
          },
          {
              id:5,
              subject:"Coding And Programming",
              location:"Bristol",
              price:7,
              spaces:5,
              image:"/assets/coding.jpg"
          },
          {
              id:6,
              subject:"Cooking And Baking",
              location:"London",
              price:8,
              spaces:5,
              image:"/assets/cooking.jpg"
          },
          {
              id:7,
              subject:"Religous Activities",
              location:"Manchester",
              price:12,
              spaces:5,
              image:"/assets/religious.jpg"
          },
          {
              id:8,
              subject:"Community Services",
              location:"Liverpool",
              price:9,
              spaces:5,
              image:"/assets/community.jpg"
          },
          {
              id:9,
              subject:"Theater",
              location:"London",
              price:13,
              spaces:5,
              image:"/assets/theater.jpg"
          },
          {
              id:10,
              subject:"Music",
              location:"Chelsey",
              price:16,
              spaces:5,
              image:"/assets/music.jpg"
          }
            ],
      }
  },
  watch: {
    cart() {
      return this.cart
    },
  },
  methods:{
    handleAddToCart(data) {
      data = JSON.parse(data)
      for (let i = 0; i < this.products.length; i++) {
          const product = this.products[i]
          if(data["item"].id == product.id){
              product.spaces--
          }
      }
      this.cart.push(data)
    },
    handleRemoveFromCart(data) {
      data = JSON.parse(data)

      const temp = []
      for (let i = 0; i < this.cart.length; i++) {
          const cart_item = this.cart[i]
          if(data["id"] != cart_item["id"]){
            temp.push(cart_item)
          }
      }

      this.cart = []

      temp.forEach(x =>{
        this.cart.push(x)
      })
      for (let i = 0; i < this.products.length; i++) {
          const product = this.products[i]
          if(data["item"].id == product.id){
              product.spaces++
          }
      }
    },
    handleOrderSubmitted(){
      this.cart = []
      this.cycle()
    },
    cycle(){
        if(this.currentView["name"] == LessonComponent["name"]){
          this.currentView = CartComponent
          this.currentViewStr = "Go Back"
          this.showProducts = false
        }else{
          this.currentView = LessonComponent
          this.currentViewStr = "Cart"
          this.showProducts = true
        }
    }
  }
}
</script>

<style>
*{
    margin: 0%;
    padding: 0%;
    box-sizing: border-box;
    font-family: 'Roboto', sans-serif;
}
button{
    padding: 0.5rem;
    border-radius: 0.5rem;
    cursor: pointer;
    border: none;
}
button:hover{
    background-color: salmon;
    color: white;
}
input{
    padding: 0.5rem;
    border-radius: 0.5rem;
}

header{
    width: 100%;
    padding: 1rem;
    display: inline-flex;
    align-self: center;
    justify-content: space-evenly;
    box-shadow: 1px 3px 3px #21214220;
}

#main_page{
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 1rem;
}
</style>
