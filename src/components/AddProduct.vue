<template>
    <v-container>
        <v-row
          align="center"
          justify="center"
        >
          <v-col cols="4">
            <v-card>
              <v-card-title>
                <h3>Add Product</h3>
              </v-card-title>
              <v-card-text>
                <v-form 
                  ref="addProductForm" >
                  <v-row>
                    <v-col cols="6">
                      <v-text-field 
                        label="Name"
                        :rules=[rules.required]                        
                        v-model="newProduct.Name">
                      </v-text-field>
                    </v-col>
                    <v-col cols="6">
                      <v-text-field 
                        label="Price"
                        type="number"
                        :rules=[rules.required]
                        v-model="newProduct.Price"
                        prefix="$"
                        @change="restrictDecimal()">
                      </v-text-field>
                    </v-col>
                    <v-col cols="12">
                      <v-select 
                        label="Category"
                        :rules=[rules.required]
                        :items="productGroups"
                        item-text="name"
                        item-value="id"
                        v-model="newProduct.GroupID">
                      </v-select>
                    </v-col>
                    <v-col cols="12">
                      <v-text-field 
                        label="Description"
                        :rules=[rules.required]   
                        v-model="newProduct.Description">
                      </v-text-field>
                    </v-col>
                  </v-row>
                </v-form>
              </v-card-text>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn 
                  class="primary"
                  @click="addProduct()"
                  :loading="loading"
                >Add Product</v-btn>
              </v-card-actions>
            </v-card>
          </v-col>
        </v-row>
    </v-container>
</template>

<script>
import axios from 'axios'
  export default {
    name: 'AddProduct',

    data: () => ({
        valid: false,
        rules:{
            required: v => !!v || 'Field Required',
            email: [
                v => !!v || 'Field required',
                v => /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(v) || 'E-mail must be valid'
            ],
            zip: [
                v => !!v || 'Field required',
                v => /\d{5}/.test(v) || 'Zip code must be 5 digits'
            ],
        },
        errorMessage: null,
        newProduct: {
            Name: null,
            Description: null,
            Price: null,
            GroupID: null,
        },
        addedProduct:null,
        productAddSuccess: null,
        loading: false,
        productGroups: null,
     }),
    mounted() {
      this.getProductGroups();
    },
    methods: {
        addProduct () {
            var self = this;
        if (self.$refs.addProductForm.validate()){
            self.loading = true,
            alert('passed validation'),
            axios.post('https://localhost:10000/products', self.newProduct)
            //This isn't waiting for the response correctly
            //Still thinks it's working when duplicate exists
            .then(()=>{
                self.productAddSuccess = true;
                self.addedProduct = self.newProduct;
                self.$refs.addProductForm.reset;
                alert('Product Added');
            })
            .catch(function(error){
                self.ProductAddSuccess = false;
                self.errorMessage = error.response.data;
                alert(self.errorMessage);
            })
            .then(
                self.loading = false,
            )
        }
        else {
            alert('no bueno');
        }
        },
        getProductGroups () {
          axios.get('https://localhost:10000/groups')
          .then(response => {
            this.productGroups = response.data;
            console.log(this.productGroups);
          })
          .catch(error => {
            console.log(error);
          })
        },
        restrictDecimal () {
          this.newProduct.Price=parseFloat(this.newProduct.Price).toFixed(2);
        }
    }
  }
</script>
