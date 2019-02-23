<template>
  <v-footer class="footer" height="auto" color="gray">
    <v-layout justify-center row wrap>
      <v-btn
        active-class="false"
        color="black"
        flat
        round
        :to="{ name: 'home' }"
        >Home</v-btn
      >
      <v-btn
        active-class="false"
        color="black"
        flat
        round
        :to="{ name: 'topStories' }"
        >Top stories</v-btn
      >
      <v-btn
        active-class="false"
        color="black"
        flat
        round
        :to="{ name: 'about' }"
        >About</v-btn
      >
      <v-btn
        active-class="false"
        color="black"
        flat
        round
        href="https://github.com/Mordeath/Vue.js-Elective"
        >GitHUb</v-btn
      >
      <v-flex py-3 text-xs-center black--text xs12
        >&copy;2019 — Veromir Mechov</v-flex
      >
    </v-layout>
  </v-footer>
</template>

<script></script>
<style scoped>
.footer {
  position: fixed;
  bottom: 0;

  width: 100%;
  line-height: 5%;
}
</style>
