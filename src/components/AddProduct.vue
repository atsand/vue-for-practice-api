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
                        @change="restrictDecimal()"
                        >
                      </v-text-field>
                    </v-col>
                    <v-col cols="12">
                      <v-select 
                        label="Category"
                        :rules=[rules.required]
                        :items="productGroups"
                        item-text="name"
                        item-value="id"
                        :loading="gettingGroups"
                        v-model="newProduct.GroupId">
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
        },
        errorMessage: null,
        newProduct: {
            Name: null,
            Description: null,
            Price: null,
            GroupId: null,
        },
        addedProduct:null,
        productAddSuccess: null,
        loading: false,
        productGroups: [{
          id: '',
          name: '',
        }],
        gettingGroups: true,
     }),
    mounted() {
      this.getProductGroups();
    },
    methods: {
        addProduct () {
            var self = this;
        if (self.$refs.addProductForm.validate()){
            self.loading = true,
            self.newProduct.Price = parseFloat(self.newProduct.Price);
            axios.post('https://localhost:10000/products', self.newProduct)
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
            alert('you must complete required fields');
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
          .finally(() => {
            this.gettingGroups = false;
          })
        },
        restrictDecimal () {
           this.newProduct.Price=parseFloat(this.newProduct.Price).toFixed(2);
        }
    }
  }
</script>
