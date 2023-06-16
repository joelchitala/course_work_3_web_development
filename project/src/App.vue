<template>
    <link href='https://unpkg.com/boxicons@2.1.4/css/boxicons.min.css' rel='stylesheet'>
  <header>
    <h1>{{sitename}}</h1>
    <button @click="cycle">{{currentViewStr}}</button>
  </header>
  <main>
    <component :is="currentView"/>
  </main>
</template>

<script>
import CartComponent from './components/Cart.vue'
import LessonComponent from './components/Lesson.vue'

export default {
  name: 'App',
  components: {
    LessonComponent
  },
  data(){
      // console.log(LessonComponent.data()["cart"]);
      return {
          sitename:"After School Activities Store",
          currentView: LessonComponent,
          currentViewStr: "Cart",
          cart:[]
      }
  },
  methods:{
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
/* #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
} */
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
