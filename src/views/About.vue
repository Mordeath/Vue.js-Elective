<template>
  <div>
    <h4
      class="text-xs-center"
      style="font-size: 30px; margin-top: 20px; margin-bottom: 10px"
    >
      Hell, my name is Veromir and this is my project for the elective
      discipline "Modern JavaScript frameworks for building client
      applications".
    </h4>
    <p style="font-size: 16px" class="text-xs-center">
      In this project i've used the New York Times API for news articles and
      Vue.js JavaScript Framework for making the application. In the project
      i've used 2 diffrent APIs.
    </p>
    <p class="text-xs-center" style="font-size: 16px">
      1 - The first one is for searching articles and it returns a maximum of 10
      result at a time.
    </p>
    <p class="text-xs-center" style="font-size: 16px">
      2- The second one is for displaying the most recent Top Stories for a
      specified section. You can check them out below.
    </p>
    <v-autocomplete
      v-bind:items="sections"
      @change="changeSection"
      v-model="section"
      label="Choose a section"
      autocomplete
      style="width: 350px; font-size: 20px; color: #555; margin: 0 auto;"
      class="nyt"
    ></v-autocomplete>
  </div>
</template>

<script>
export default {
  data() {
    return {
      sections: [
        "Home",
        "Opinion",
        "World",
        "National",
        "Politics",
        "Upshot",
        "Business",
        "Technology",
        "Science",
        "Health",
        "Sports",
        "Arts",
        "Books",
        "Movies",
        "Theater",
        "Sunday Review",
        "Fashion",
        "T Magazine",
        "Food",
        "Travel",
        "Magazine",
        "Real Eastate",
        "Automobiles",
        "Insider"
      ]
    };
  }
};
</script>

<style></style>
