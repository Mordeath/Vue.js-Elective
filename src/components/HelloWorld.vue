/* eslint-disable prettier/prettier */
<template>
  <div>
    <h1
      class="text-xs-center"
      style="font-size: 50px; margin-top: 20px; margin-bottom: 10px"
    >
      Search Articles
    </h1>
    <v-form v-on:submit.prevent="onSubmit">
      <div style="margin: 0 auto; text-align: center; margin-bottom: 50px">
        <div style="margin-bottom: 40px; margin: 0 auto;">
          <v-text-field
            v-on:click="clear"
            v-model="search"
            class="mx-1"
            flat
            label="Search Articles"
            prepend-inner-icon="search"
          ></v-text-field>
          <v-btn v-on:click="searchArticles(search)">Search</v-btn>
        </div>
      </div>
    </v-form>

    <div class="loader">
      <!-- Spinner -->
      <span v-if="loading" class="nyt">
        <pulse-loader style="margin-top: 10px" color="red"></pulse-loader>
      </span>
    </div>

    <div v-if="errors">
      <h1 class="nyt" style="color: red">Error fetching data</h1>
    </div>

    <v-layout row wrap class="full-height" v-if="!errors">
      <v-flex xs12 sm4 md4 v-for="result in results" :key="result.uuid">
        <v-layout column>
          <v-flex d-flex>
            <v-card
              :target="setTarget()"
              :href="result.web_url"
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
                    <p class="nyt">{{ result.headline.main }}</p>
                  </h3>
                  <div>
                    <p>{{ result.snippet }}</p>
                  </div>
                  <div style="margin-bottom: 50px">
                    <span class="grey--text">{{
                      makeFriendlyDate(result.pub_date)
                    }}</span>
                  </div>
                </div>
              </v-card-title>
            </v-card>
          </v-flex>
        </v-layout>
      </v-flex>
    </v-layout>
    <h2 style="text-align: cente" v-if="!isHidden">
      No articles foud for "{{ search }}"!
    </h2>
  </div>
</template>

<script>
import PulseLoader from "vue-spinner/src/PulseLoader.vue";
import key from "./api/nyt_api";
import axios from "axios";
import uuidv4 from "uuid/v4";
import dateFormat from "dateformat";

export default {
  mounted() {
    this.search = "Home";
    this.fetchArticles(this.search);
  },

  methods: {
    makeFriendlyDate: function(date) {
      // return dateFormat(d, "dddd, mmmm dS, yyyy, h:MM:ss TT");
      return dateFormat(date, "dd.mm.yyyy,  h:MM TT");
    },
    setTarget: function() {
      return this.enabled ? "_blank" : "_self";
    },
    clear: function() {
      this.search = "";
    },

    searchArticles: function(q) {
      this.search = q;
      this.loading = true;
      this.fetchArticles(this.search);
    },
    fetchArticles: function(search) {
      let vm = this;
      search = search.toLowerCase().replace(/\s/g, "");
      console.log(search);
      let url = `https://api.nytimes.com/svc/search/v2/articlesearch.json?q=${search}&api-key=${
        key.apiKey
      }`;

      axios
        .get(url)
        .then(function(response) {
          console.log(response);
          vm.loading = false;
          vm.results = response.data.response.docs;
          if (vm.results.length == 0) {
            console.log("Няма никой вкъщи!");
            vm.isHidden = false;
          }
          vm.processPosts();
        })
        .catch(function(error) {
          vm.errors = true;
          console.log(error);
          vm.$forceUpdate();
        });
    },
    processPosts: function() {
      var posts = this.results;
      let imgURL = "https://static01.nyt.com/";
      posts.map(post => {
        let imgObj = post.multimedia.find(media => media.subType === "blog533");
        console.log(imgObj);
        post.image_url = imgObj
          ? imgURL + imgObj.url
          : "https://placehold.it/300x200?text=N/A";
        post.uuid = uuidv4();
      });
      this.results = posts;
      console.log(this.results);
    }
  },
  computed: {},
  components: {
    PulseLoader
  },

  data() {
    return {
      loading: true,
      errors: false,
      search: String,
      enabled: true,
      isHidden: true,
      lastUpdated: null,
      results: []
    };
  }
};
</script>

<style></style>
