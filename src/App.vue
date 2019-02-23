<template>
  <v-app id="myApp">
    <div id="app">
      <app-header></app-header>
      <v-container>
        <v-layout>
          <v-flex>
            <router-view />
          </v-flex>
        </v-layout>
      </v-container>
      <app-footer></app-footer>
    </div>
  </v-app>
</template>

<script>
import Footer from "./components/Footer";

import Header from "./components/Header.vue";

export default {
  components: {
    "app-header": Header,
    "app-footer": Footer
  }
};
</script>

<style></style>
