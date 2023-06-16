<template>
    <h1>Cart Page</h1>
    
    <div id="Checkout">
        <h2>Checkout Page</h2>
        <button style="font-size: 1.15rem;" v-on:click="showCheckout">
            <i class='bx bx-left-arrow-alt'></i>
            Go back
        </button>
        <div>
            <div class="input_container">
                <label for="name">Name: </label>
                <input type="text" v-model.trim="user_data.name">
            </div>
            <div class="input_container">
                <label for="phone" pattern="[A-Za-z]">Phone: </label>
                <input type="text" v-model.trim="user_data.phone">
            </div>
            <button v-if="canCheckout">Checkout</button>
        </div>
    </div>
    <div id="cart_container">
        <h2>Cart({{cartItemCount}})</h2>
        <div id="products">
            <div class="product" for="item in cart" id="item.id">
                <!-- <img class="image" v-bind:src="item.item.image">
                <div class="info_container">
                    <p>Subject: {{item.item.subject}}</p>
                    <p>Location: {{item.item.location}}</p>
                    <p>Price: {{item.item.price}}</p>
                    <button  v-on:click="remove_from_cart(item)">Remove From Cart</button>
                </div> -->
            </div>
        </div>
    </div>
    <div id="info" style="display: none;">
        <h2>Checkout</h2>

        <button style="font-size: 1.15rem;" v-on:click="showCheckout">
            <i class='bx bx-left-arrow-alt'></i>
            Go back
        </button>
    </div>
</template>
  
  <script>
//   import AppVue from '../App.vue'
// import App from '@/App.vue';

  export default {
    name: 'CartComponent',
    props:{
        cartItems: {
            type: Array,
            // default: () => [],
        }
    },
    created(){
        console.log(this.cartItems);
    },
    computed:{
            cartItemCount(){
                return this.cart.length || "Empty";
            },
            
            canRemoveFromCart(){
                return this.cart.length > 0
            },
            canCheckout(){
                const letters = /^[A-Za-z]+$/;
                const numbers = /^[0-9]+$/;

                let valid = true
                if(this.user_data.name.match(letters) == null){
                    valid = false
                }

                if(this.user_data.phone.match(numbers) == null){
                    valid = false
                }

                if(valid){
                    return true
                }else{
                    return false
                }
            }
        
        },
        methods:{
            remove_from_cart(cart_item){
                // for (let i = 0; i < products.length; i++) {
                //     const item = products[i]
                //     if(item.id == cart_item.item.id){
                //         item.spaces++
                //     }
                // }
                for(let i = 0; i < this.cart.length; i++){
                    const item = this.cart[i]
                    if(cart_item.id == item.id){
                        this.cart.splice(i,1)
                        break;
                    }
                }
                
            },
            
        },
        data(){
            return{
                cart:[],
                user_data:{
                    name:"",
                    phone:"",
                }
            }
        }
  }
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
  
  </style>
  