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
                        ${{ product.price }}
                    </div>
                </v-card-text>
                <v-spacer></v-spacer>
                <v-card-actions class="flex-end">
                    <v-spacer></v-spacer>
                    <v-btn
                        class="primary"
                        @click="addToCart(product.id)"
                        v-if="!cart.filter(p => p.id == product.id).length > 0 && product.isActive"
                    >
                        Add To Cart
                    </v-btn>
                    <v-btn
                        class="success"
                        @click="removeFromCart(product.id)"
                        v-if="cart.filter(p => p.id == product.id).length > 0 && product.isActive"
                    >
                        Remove
                    </v-btn>
                    <v-btn
                        class="secondary"
                        disabled
                        v-if="!product.isActive"
                    >
                        Unavailable
                    </v-btn>
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
        removeFromCart(id){
            this.cart = this.cart.filter(x => x.id !== id);
        }
    },
  }
</script>

<style scoped>
.product-card{
    max-height: 500px;
}
</style>
