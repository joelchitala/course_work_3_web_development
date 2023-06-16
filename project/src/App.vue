<template>
    <link href='https://unpkg.com/boxicons@2.1.4/css/boxicons.min.css' rel='stylesheet'>
  <header>
    <h1>{{sitename}}</h1>
    <button @click="cycle" >{{currentViewStr}}</button>
  </header>
  <main>
    <component :is="currentView"   @click="handleAddToCart"/>
    <!-- <LessonComponent @add-cart-event="HandleCartEvents"></LessonComponent> -->
  </main>
</template>

<script>
import CartComponent from './components/Cart.vue'
import LessonComponent from './components/Lesson.vue'

export default {
  name: 'App',
  
  data(){
      // console.log(LessonComponent.data()["cart"]);
      return {
          sitename:"After School Activities Store",
          currentView: LessonComponent,
          currentViewStr: "Cart",
          cart:[],
          products:[
              {
                id:1,
                subject:"Art And Craft",
                location:"London",
                price:22,
                spaces:5,
                image:"art_and_craft.jpg"
            },
            {
                id:2,
                subject:"Sport",
                location:"Oxford",
                price:19,
                spaces:5,
                image:"sport.jpg"
            },
            {
                id:3,
                subject:"Music",
                location:"London",
                price:20,
                spaces:5,
                image:"music.jpg"
            },
            {
                id:4,
                subject:"Theater",
                location:"York",
                price:17,
                spaces:5,
                image:"theater.jpg"
            },
            {
                id:5,
                subject:"Coding And Programming",
                location:"Bristol",
                price:7,
                spaces:5,
                image:"coding.jpg"
            },
            {
                id:6,
                subject:"Cooking And Baking",
                location:"London",
                price:8,
                spaces:5,
                image:"cooking.jpg"
            },
            {
                id:7,
                subject:"Religous Activities",
                location:"Manchester",
                price:12,
                spaces:5,
                image:"religious.jpg"
            },
            {
                id:8,
                subject:"Community Services",
                location:"Liverpool",
                price:9,
                spaces:5,
                image:"community.jpg"
            },
            {
                id:9,
                subject:"Theater",
                location:"London",
                price:13,
                spaces:5,
                image:"theater.jpg"
            },
            {
                id:10,
                subject:"Music",
                location:"Chelsey",
                price:16,
                spaces:5,
                image:"music.jpg"
            }
          ],
      }
  },
  methods:{
    handleAddToCart(data) {
      console.log(data);
      this.cart.push(JSON.parse(data));
    },
    cycle(){
        if(this.currentView["name"] == LessonComponent["name"]){
          this.currentView = CartComponent
          this.currentViewStr = "Go Back"
        }else{
          this.currentView = LessonComponent
          this.currentViewStr = "Cart"
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
