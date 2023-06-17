<template>
  <h1>Lesson Page</h1>
  <section style="display: flex; flex-direction: row; width: 100%;">
    <section style="width: 30%;">
        <h3>Filter & Order</h3>
        <!-- div element to encapsulate the sorting div and order div -->
        <div id="filters" style="display:flex; flex-direction: column; align-self: flex-start; width: 100%;">
            <h3 style="margin: 0.5rem 0;">Filter</h3>
            <!-- div element to encapsulate the sorting input radios -->
            <div id="sort">
                <div v-for="(value, key) in sort_dict" :key="value.id">
                  
                    <input type="radio" name="sort" value="key" v-if="value.checked" checked v-on:click="sort(key)">
                    <input type="radio" name="sort" value="key" v-else v-on:click="sort(key)">
                    <label for="sort">{{key}}</label>
                </div>
            </div>
            <h3 style="margin: 0.5rem 0;">Order</h3>
            <!-- div element to encapsulate the order input radios -->
            <div id="order">
                <div v-for="(value, key) in order_dict" :key="value.id">
                  
                    <input type="radio" name="order" value="key" v-if="value.checked" checked v-on:click="order(key)">
                    <input type="radio" name="order" value="key" v-else v-on:click="order(key)">
                    <label for="order">{{key}}</label>
                </div>
            </div>
        </div>
    </section>

    <section style="width: 100%;">
      <div style="width: 100%; display: inline-flex; align-items: center; justify-content: flex-start;">
          <!-- <div style="align-self: flex-end; margin-right: 2rem;">
              <button style="font-size: 1.15rem;" v-if="canRemoveFromCart" v-on:click="showCheckout">
                  {{cartItemCount}}
                  <i class="bx bx-cart"></i>
                  Checkout
              </button>
              <button style="font-size: 1.15rem;" disabled="disabled" v-else>
                  {{cartItemCount}}
                  <i class="bx bx-cart"></i>
              </button>
          </div> -->
          <div id="search">
              <input type="text" v-bind:placeholder="searchTerm" v-if="search" v-model="searchInput">
              <button v-on:click="search"><i class='bx bx-search'></i></button>
          </div>
          
      </div>
      
      <div id="products">
          <div class="product" v-for="item in product_copy" :key="item.id">
            <img class="image" :src="item.image">
            <div class="info_container">
                <p>Subject: {{item.subject}}</p>
                <p>Location: {{item.location}}</p>
                <p>Price: ${{item.price}}</p>
                <p>Spaces: {{item.spaces}}</p>
                <button v-if="item.spaces > 0" @click="add_to_cart(item)">Add To Cart</button>
                <button v-else disabled>Add To Cart</button>
            </div>
          </div>
      </div>
  </section>
    
</section>
</template>

<script>
// import AppVue from '../App.vue'

// const capitalize = (str) =>{
//     if(str != '' & typeof(str) == "string"){
//         let temp_str = str[0].toUpperCase()
//         for(let i = 1; i < str.length; i++){
//             temp_str += str[i] 
//         }

//         return temp_str
//     }
//     return null
// }

export default {
  name: 'LessonComponent',
  props:{
        productItems: {
            type: Array,
            // default: () => [],
        }
    },
  emits: ['add-cart'],
  created(){
    this.loadProducts()
  },
  methods:{
    
    showCheckout(){
        this.showProducts = this.showProducts ? false : true
    },
    add_to_cart(item){
        const id = `${item.subject[0]}`+item.id+`${item.location[0]}`+Math.ceil(Math.random()*1000000)
        let data = {
            "id": id,
            "item":item,
        }
        this.$emit("add-cart",JSON.stringify(data))
    },
    search_func(query){
        let val = this.sort_value.toLowerCase()

        if(val != "all" & query.trim() != ''){

            if(val != "price"){
                this.product_copy = this.productItems.filter(x => x[val].toLowerCase().includes(query.toLowerCase()))
            }else{
                if(query.match(/^[0-9]+$/) != null){
                    this.product_copy = this.productItems.filter(x => (''+x[val]).includes(query))
                }
            }
            this.order_sort(this.sort_value)
        }

        
        if(query.trim() == ''){
            this.loadProducts()
            this.order_sort(this.sort_value)
        }

    },
    order(value){
        this.order_value = value
        this.order_sort(this.sort_value)
    },
    sort(value){
        this.sort_value = value
        this.order_sort(value)
    },
    order_sort(criteria){
        criteria = criteria.toLowerCase()
        if(criteria != "all"){
            this.searchTerm = criteria
            const isAscending = this.order_value == "Ascending"
            this.product_copy.sort((a,b)=>{
                if(a[criteria] > b[criteria]) return isAscending ? 1 : -1
                if(b[criteria] > a[criteria]) return isAscending ? -1 : 1                       
                return 0
            })
        }else{
            this.searchTerm = "Please choose criteria eg. subject, location, price."
            this.loadProducts()
        }
    },
    loadProducts() {
      this.product_copy = []
    
      this.productItems.forEach(x=>{
        this.product_copy.push(x);
      })
    }
  },
  data(){
    return{
      sitename:"After School Activities Store",
      cart:[],
      order_value:"Ascending",
      order_dict:{
          "Ascending":{id:1,checked:true},
          "Descending":{id:2,checked:false}
      },
      sort_value:"All",
      sort_dict:{
          "All":{id:1,checked:true},
          "Subject":{id:2,checked:false},
          "Location":{id:3,checked:false},
          "Price":{id:4,checked:false}
      },
      searchInput:'',
      searchTerm:'Please choose criteria eg. subject, location, price.',
      
      product_copy:[],
      user_data:{
          name:"",
          phone:"",
      },
    }
  },
  computed:{
    cartItemCount(){
        return this.cart.length || "Empty";
    },
    canRemoveFromCart(){
        return this.cart.length > 0
    },
    search(){
        this.search_func(this.searchInput)
        return true
    },
  }
}
</script>

<style scoped>
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
</style>
