<template>
  <v-container>
      <v-row>
          <v-col v-for="product in allProducts" :key="product.id"
                cols="2">
            <v-card
                class="product-card"
            >
                <v-card-title>
                    {{ product.name}}
                </v-card-title>
                <v-card-text>
                    <div>
                        {{ product.description }}
                    </div>
                    <div>
                        {{ product.price }}
                    </div>
                </v-card-text>
                <v-spacer></v-spacer>
                <v-card-actions class="flex-end">
                    <v-spacer></v-spacer>
                    <v-btn
                        class="primary"
                        right
                        @click="addToCart(product.id)"
                        v-if="!cart.filter(p => p.id == product.id).length > 0 && product.isActive"
                    >
                        Add To Cart
                    </v-btn>
                    <v-btn
                        class="secondary"
                        disabled
                        v-if="!product.isActive"
                    >
                        Unavailable
                    </v-btn>
                    <v-text-field 
                        v-if="cart.filter(p => p.id == product.id).length > 0"
                        type="number" 
                        append-outer-icon="mdi-plus"
                        @click:append-outer="increment(product.id)" 
                        prepend-icon="mdi-minus" 
                        @click:prepend="decrement(product.id)"
                    ></v-text-field>
                </v-card-actions>
            </v-card>
          </v-col>
      </v-row>
  </v-container>
</template>

<script>
  import axios from 'axios'

  export default {
    name: 'ViewProducts',

    data: () => ({
        allProducts: null,
        cart: [],
        foo: null,
    }),
    mounted (){
        this.getAllProducts();
    },
    methods: {
        increment(id){
            var index = this.cart.findIndex((product => product.id == id));
            if(index >= 0){
                this.cart[index].qty = this.cart[index].qty + 1;
                alert(this.cart[index].qty)
            }
            console.log(this.cart.filter(p => p.id == id))
        },
        decrement(id){
            var index = this.cart.findIndex((product => product.id == id));
            if(index && this.cart[index].qyt >= 1){
                this.cart[index].qty = this.cart[index] --;
            }
        },
        getAllProducts(){
            axios.get('https://localhost:10000/products/all')
            .then(response => {
                this.allProducts = response.data;
                console.log(this.allProducts);
            })
            .catch(error => {
                console.log(error.response.data)
            })
        },
        addToCart(id){
            this.cart.push({id: id, qty: 1});
            console.log(this.cart);
        },
    },
  }
</script>

<style scoped>
.product-card{
    min-height: 300px;
}
</style>
