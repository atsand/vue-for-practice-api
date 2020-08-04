<template>
    <v-container>
        <v-card
            min-height="500"
        >
            <v-card-title>
                Product Groups
            </v-card-title>
            <v-card-text>
                <v-data-table
                    v-if="allGroups"
                    :headers="headers"
                    :items="allGroups"
                ></v-data-table>
            </v-card-text>
        </v-card>
    </v-container>
</template>

<script>
import axios from 'axios'

  export default {
    name: 'ViewProductGroups',

    data: () => ({
        allGroups: null,
        headers: [
            {text: 'Name', value: 'name'},
        ],
        search: null,
    }),
    methods: {
        getAllGroups () {
            axios.get('https://localhost:10000/groups')
            .then((response)=>{
                this.allGroups = response.data;
            })
        },
    },
    mounted() {
        this.getAllGroups();
    },
  }
</script>
