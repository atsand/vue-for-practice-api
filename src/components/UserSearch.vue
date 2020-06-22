<template>
    <v-container>
        <v-card
            min-height="500"
        >
            <v-card-title>
                Users
                <v-spacer></v-spacer>
                <v-text-field
                    v-model="search"
                    label="Search"
                    prepend-icon="mdi-magnify"
                    single-line
                    hide-details
                    clearable
                >
                </v-text-field>
            </v-card-title>
            <v-card-text>
                <v-data-table
                    v-if="allUsers"
                    :headers="headers"
                    :items="allUsers"
                    :search="search"
                    multi-sort
                ></v-data-table>
            </v-card-text>
            <!-- <v-card-actions>
                <v-btn
                    class="primary"
                    @click="getAllUsers()"
                >
                    Get All Users
                </v-btn>
            </v-card-actions> -->
        </v-card>
        <div v-for="user in allUsers" :key="user.id">{{ user }}</div>
    </v-container>
</template>

<script>
import axios from 'axios'

  export default {
    name: 'UserSearch',

    data: () => ({
        allUsers: null,
        headers: [
            {text: 'First', value: 'name.first'},
            //{text: 'Middle', value: 'name.middle'},
            {text: 'Last', value: 'name.last'},
            {text: 'Email', value: 'email'},
            {text: 'Street', value: 'address.street'},
            {text: 'State', value: 'address.state'},
            {text: 'Zip Code', value: 'address.zipCode'},
        ],
        search: null,
    }),
    methods: {
        getAllUsers () {
            axios.get('https://localhost:10000/user')
            .then((response)=>{
                this.allUsers = response.data;
            })
        },
    },
    mounted() {
        this.getAllUsers();
    },
  }
</script>
