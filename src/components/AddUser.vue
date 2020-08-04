<template>
    <v-container>
        <v-row
          align="center"
          justify="center"
        >
          <v-col cols="4">
            <v-card>
              <v-card-title>
                <h3>Add User</h3>
              </v-card-title>
              <v-card-text>
                <v-form 
                  ref="addUserForm" >
                  <v-row>
                    <v-col cols="4">
                      <v-text-field 
                        label="First Name"
                        :rules=[rules.required]                        
                        v-model="newUser.Name.Last">
                      </v-text-field>
                    </v-col>
                    <v-col cols="4">
                      <v-text-field 
                        label="Middle Name"
                        v-model="newUser.Name.Middle">
                      </v-text-field>
                    </v-col>
                    <v-col cols="4">
                      <v-text-field 
                        label="Last Name"
                        :rules=[rules.required] 
                        v-model="newUser.Name.First">
                      </v-text-field>
                    </v-col>
                  </v-row>
                  <v-row>
                    <v-col cols="12">
                      <v-text-field
                        label="Street"
                        :rules=[rules.required] 
                        v-model="newUser.Address.Street">
                      </v-text-field>
                    </v-col>
                  </v-row>
                  <v-row>
                    <v-col cols="6">                      
                      <v-text-field
                        label="City"
                        :rules=[rules.required] 
                        v-model="newUser.Address.City">
                      </v-text-field>
                    </v-col>
                    <v-col cols="6">                      
                      <v-text-field
                        label="State"
                        :rules=[rules.required] 
                        v-model="newUser.Address.State">
                      </v-text-field>
                    </v-col>
                  </v-row>
                  <v-row>
                    <v-col cols="6">
                      <v-text-field
                        label="Zip Code"
                        :rules="rules.zip" 
                        v-model="newUser.Address.ZipCode">
                      </v-text-field>
                    </v-col>
                    <v-col cols="6">                      
                      <v-text-field
                        label="Email"
                        :rules="rules.email"
                        v-model="newUser.Email">
                      </v-text-field>
                    </v-col>
                  </v-row>
                </v-form>
              </v-card-text>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn 
                  class="primary"
                  @click="addUser()"
                  :loading="loading"
                >Add User</v-btn>
              </v-card-actions>
            </v-card>
          </v-col>
        </v-row>
    </v-container>
</template>

<script>
import axios from 'axios'
  export default {
    name: 'AddUser',

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
        newUser: {
            Address: {
                Street: null,
                City: null,
                State: null,
                ZipCode: null,
            },
            Name: {
                First: null,
                Middle: '',
                Last: null,
            },
            Email: null,
        },
        addedUser:null,
        userAddSuccess: null,
        loading: false,
     }),
    methods: {
        addUser () {
            var self = this;
          if (self.$refs.addUserForm.validate()){
              self.loading = true,
              axios.post('https://localhost:10000/user', self.newUser)
              .then(()=>{
                  self.userAddSuccess = true;
                  self.addedUser = self.newUser;
                  self.$refs.addUserForm.reset();
                  alert('User Added');
              })
              .catch(function(error){
                  self.userAddSuccess = false;
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
        }
    }
  }
</script>
