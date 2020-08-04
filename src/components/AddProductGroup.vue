<template>
    <v-container>
        <v-row
          align="center"
          justify="center"
        >
          <v-col cols="4">
            <v-card>
              <v-card-title>
                <h3>Add Product Group</h3>
              </v-card-title>
              <v-card-text>
                <v-form 
                  ref="addProductGroupForm" >
                  <v-row>
                    <v-col cols="12">
                      <v-text-field 
                        label="Group Name"
                        :rules=[rules.required]                        
                        v-model="newProductGroup.name">
                      </v-text-field>
                    </v-col>
                  </v-row>
                </v-form>
              </v-card-text>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn 
                  class="primary"
                  @click="addProductGroup()"
                  :loading="loading"
                >Add Group</v-btn>
              </v-card-actions>
            </v-card>
          </v-col>
        </v-row>
    </v-container>
</template>

<script>
import axios from 'axios'
  export default {
    name: 'AddProductGroup',

    data: () => ({
        valid: false,
        rules:{
            required: v => !!v || 'Field Required',
        },
        errorMessage: null,
        newProductGroup: {
            Name: null,
        },
        addedProductGroup:null,
        groupAddSuccess: null,
        loading: false,
     }),
    mounted() {
    },
    methods: {
        addProductGroup () {
            var self = this;
          if (self.$refs.addProductGroupForm.validate()){
              self.loading = true,
              axios.post()
              .then(()=>{
                  self.groupAddSuccess = true;
                  alert('Group Added');
              })
              .catch(function(error){
                  self.groupAddSuccess = false;
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
    }
  }
</script>
