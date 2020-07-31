<template>
  <v-app id="inspire">
    <v-navigation-drawer
      v-model="drawer"
      app
      clipped
    >
    <!--Items in nav drawer-->
    <!-- Clean up and add links to object array in data -->
      <v-list dense>
        <v-list-item link @click="activeComponent = 'Home'">
          <v-list-item-action>
            <v-icon>mdi-home</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>Home</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        <v-list-item link @click="activeComponent = 'AddUser'">
          <v-list-item-action>
            <v-icon>mdi-account-plus</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>Add User</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        <v-list-item link @click="activeComponent = 'UserSearch'">
          <v-list-item-action>
            <v-icon>mdi-account-search</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>Find User</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        <v-list-item link @click="activeComponent = 'AddProduct'">
          <v-list-item-action>
            <v-icon>mdi-view-grid-plus</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>Add Product</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        <v-list-item link @click="activeComponent = 'ViewProducts'">
          <v-list-item-action>
            <v-icon>mdi-store</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>View Products</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <!--Top bar-->
    <v-app-bar
      app
      clipped-left
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
      <v-toolbar-title>Rob's Place</v-toolbar-title>
    </v-app-bar>

    <!--Main content-->
    <v-main>
      <v-container
        fluid
      >
        <Home v-if="activeComponent == 'Home'"></Home>
        <AddUser v-if="activeComponent == 'AddUser'"></AddUser>
        <UserSearch v-if="activeComponent == 'UserSearch'"></UserSearch>
        <ViewProducts v-if="activeComponent == 'ViewProducts'"></ViewProducts>
        <AddProduct v-if="activeComponent == 'AddProduct'"></AddProduct>
      </v-container>
    </v-main>

    <!--Footer-->
    <v-footer app>
      <span>&copy;</span>
      <span v-text="new Date().getFullYear()"></span>
    </v-footer>
  </v-app>
</template>

<script>
import Home from './components/Home';
import AddUser from './components/AddUser'
import UserSearch from './components/UserSearch'
import ViewProducts from './components/ViewProducts'
import AddProduct from './components/AddProduct'

export default {
  name: 'App',
  components: {
    AddUser, Home, UserSearch, ViewProducts, AddProduct,
  },
  data: () => ({
    activeComponent: 'Home',
    drawer: null,
  }),
};
</script>
