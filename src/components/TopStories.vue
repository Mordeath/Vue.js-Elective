<template>
  <v-container fluid grid-list-sm style="margin-bottom: 45px">
    <!-- <div style="clear: both"></div> -->
    <h1
      class="text-xs-center nyt"
      style="font-size: 50px; margin-top: 10px; margin-bottom: 10px"
    >
      Top News Stories
    </h1>

    <div style="margin: 0 auto; text-align: center; margin-bottom: 50px">
      <div style="margin-bottom: 40px; margin: 0 auto;">
        <v-autocomplete
          v-bind:items="sections"
          @change="changeSection"
          v-model="section"
          label="Choose a section"
          autocomplete
          style="width: 350px; font-size: 30px; color: #555; margin: 0 auto;"
          class="nyt"
        ></v-autocomplete>
        <div style="margin-top: -10px; font-size: 15px; color: black">
          Updated: {{ lastUpdated }}
        </div>
      </div>

      <div class="loader">
        <!-- Spinner -->
        <span v-if="loading" class="nyt">
          <pulse-loader style="margin-top: 10px" color="red"></pulse-loader>
        </span>
      </div>

      <div v-if="errors">
        <h1 class="nyt" style="color: red">Error fetching data</h1>
      </div>
    </div>

    <div style="clear: both"></div>

    <v-layout row wrap class="full-height" v-if="!errors">
      <v-flex xs12 sm4 md4 v-for="result in results" :key="result.uuid">
        <v-layout column>
          <v-flex d-flex>
            <v-card
              :target="setTarget()"
              :href="result.url"
              class="post"
              style="margin: 2px;"
              color="LightCoral  darken-3"
              height="500px"
              tile
              raised
            >
              <v-img :src="result.image_url" height="200px"></v-img>
              <v-card-title primary-title>
                <div>
                  <h3 class="headline mb-0">
                    <p class="nyt">{{ result.title }}</p>
                  </h3>
                  <h5>{{ result.type_of_material }}</h5>

                  <div>
                    <p>{{ result.abstract }}</p>
                  </div>

                  <div style="margin-bottom: 50px">
                    <span class="grey--text">
                      {{ makeFriendlyDate(result.published_date) }}
                    </span>
                  </div>
                </div>
              </v-card-title>
            </v-card>
          </v-flex>
        </v-layout>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import PulseLoader from "vue-spinner/src/PulseLoader.vue";
import key from "./api/nyt_api";
import axios from "axios";
import uuidv4 from "uuid/v4";
import dateFormat from "dateformat";
//import Switches from "vue-switches";

export default {
  mounted() {
    this.section = "Home";
    this.fetchArticles(this.section);
  },

  methods: {
    makeFriendlyDate: function(d) {
      // return dateFormat(d, "dddd, mmmm dS, yyyy, h:MM:ss TT");
      return dateFormat(d, "dd.mm.yyyy,  h:MM TT");
    },
    setTarget: function() {
      return this.enabled ? "_blank" : "_self";
    },
    changeSection: function(e) {
      this.section = e;
      this.loading = true;
      this.fetchArticles(this.section);
    },
    fetchArticles: function(section) {
      let vm = this;
      section = section.toLowerCase().replace(/\s/g, "");
      console.log(section);
      let url = `https://api.nytimes.com/svc/topstories/v2/${section}.json?api-key=${
        key.apiKey
      }`;
      axios
        .get(url)
        .then(function(response) {
          console.log(response);
          vm.loading = false;
          vm.results = response.data.results;
          vm.lastUpdated = dateFormat(
            response.data.last_updated,
            "dd.mm.yyyy,  h:MM:ss TT"
          );
          vm.processPosts();
        })
        .catch(function(error) {
          vm.errors = true;
          console.log(error);
          vm.$forceUpdate();
        });
    },
    processPosts: function() {
      let posts = this.results;
      // Add extra attributes: image URL and uuids for the keys.
      posts.map(post => {
        let imgObj = post.multimedia.find(
          media => media.format === "superJumbo"
        );
        post.image_url = imgObj
          ? imgObj.url
          : "https://placehold.it/300x200?text=N/A";
        post.uuid = uuidv4();
      });
      this.results = posts;
    }
  },
  computed: {
    myApiKey: function() {
      return process.api.apiKey;
    }
  },
  components: {
    PulseLoader
  },

  data() {
    return {
      section: "Home",
      loading: true,
      errors: false,
      enabled: true,
      lastUpdated: null,
      results: [],
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
