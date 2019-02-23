<template>
  <div>
    <v-toolbar>
      <div height="60px">
        <v-img :src="image" width="60px" style="border-radius: 5%"></v-img>
      </div>
      <v-toolbar-title>Nothing But Fake News</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-toolbar-items class="hidden-sm-and-down">
        <v-btn :to="{ name: 'home' }" exact>Search</v-btn>
        <v-btn :to="{ name: 'topStories' }">Top stories</v-btn>
        <v-btn :to="{ name: 'about' }">About</v-btn>
      </v-toolbar-items>
    </v-toolbar>
  </div>
</template>

<script>
import image from "../assets/profile.png";
export default {
  data() {
    return {
      image: image
    };
  }
};
</script>

<style></style>
