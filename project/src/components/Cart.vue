<template>
    <section style="display: inline-flex; align-items: start;">
        <div id="Checkout">
            <h2>Checkout Page</h2>
            <div>
                <div class="input_container">
                    <label for="name">Name: </label>
                    <input type="text" v-model.trim="user_data.name">
                </div>
                <div class="input_container">
                    <label for="phone" pattern="[A-Za-z]">Phone: </label>
                    <input type="text" v-model.trim="user_data.phone">
                </div>
                <button v-if="canCheckout" @click="checkout()">Checkout</button>
            </div>
        </div>
        <div id="cart_container">
            <h2>Cart({{cartItemCount}})</h2>
            <div id="products">
                <div class="product" v-for="item in getCartItems" :key="item.id">
                    <img class="image" v-bind:src="item.item.image">
                    <div class="info_container">
                        <p>Subject: {{item.item.subject}}</p>
                        <p>Location: {{item.item.location}}</p>
                        <p>Price: {{item.item.price}}</p>
                        <button  v-on:click="remove_from_cart(item)">Remove From Cart</button>
                    </div>
                </div>
            </div>
        </div>

    </section>
</template>
  
  <script>
//   import AppVue from '../App.vue'
// import App from '@/App.vue';

  export default {
    name: 'CartComponent',
    props:{
        cartItems: {
            type: Array,
        }
    },
    emits: ['remove-cart','order-submitted'],
    computed:{
        getCartItems(){
            return this.cartItems
        },
        cartItemCount(){
            return this.cartItems.length || "Empty";
        },
        
        canRemoveFromCart(){
            return this.cartItems.length > 0
        },
        canCheckout(){
            const user = this.user_data
            return user.name.match(/^[A-Za-z\s]+$/) && user.phone.match(/^[0-9]+$/)
        }
    },
    methods:{
        remove_from_cart(data){
            this.$emit("remove-cart",JSON.stringify(data))
        },
        checkout(){
            alert("Order submited")
            this.$emit("order-submitted")
        }
    },
    data(){
        return{
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
  #app{
    width: 100%;
    height: 100%;
}

header{
    width: 100%;
    padding: 1rem;
    box-shadow: 1px 3px 3px #21214220;
}
#main_page{
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 1rem;
}
#filters div{
    margin: 0 0.5rem;
}
#search{
    width: 30%;
    height: 45px;
    display: inline-flex;
    align-items: center;
    background: #f5f5f5;
    padding: 0;
    border-radius: 0.5rem;
    padding: 0.5rem;
}
#search:focus{
    border: 1px dashed #212142;
}
#search input{
    width: 100%;
    border-radius: 0%;
    border: none;
    background: transparent;
    outline: none;
}
#search i{
    font-size: 1.25rem;
    cursor: pointer;
}
#products{
    width: 90%;
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    grid-row-gap: 0.5rem;
    grid-column-gap: 0.5rem;
}
.product{
    /* padding: 1.5rem; */
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    box-shadow: 1px 3px 3px #21214225;
}
.product .info_container{
    padding: 0.5rem;
    
}
.product button{
    width: 100%;
    padding: 0.75rem 0;
}
.product img{
    width: 100%;
    object-fit: cover;
}
.product *{
    margin: 0.25rem 0;
}
#cart_page{
    width: 100%;
    height: 100%;
    padding: 1rem;
    display: flex;
    flex-direction: row;
}
#cart_page #Checkout{
    width: 50%;
    height: 100%;
}
#cart_page #Checkout button{
    padding: 0.75rem;
}
#cart_page #Checkout *{
    margin: 0.25rem 0;
}

#cart_page #cart_container{
    width: 100%;
    height: 100%;
}
  </style>
  