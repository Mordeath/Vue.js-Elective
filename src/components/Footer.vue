<template>
  <v-footer class="footer" height="auto" color="gray">
    <v-layout justify-center row wrap>
      <v-btn v-for="link in links" :key="link" color="black" flat round>
        {{ link }}
      </v-btn>
      <v-flex danger py-1 text-xs-center black--text xs10
        >&copy;2019 —
        <P style="padding-left: 165px">Veromir Mechov</P>
      </v-flex>
    </v-layout>
  </v-footer>
</template>

<script>
export default {
  data: () => ({
    links: ["Home", "About Us", "Team", "Contact Us"]
  })
};
</script>
<style scoped>
.footer {
  position: fixed;
  bottom: 0;
  width: 100%;
  line-height: 5%;
}
</style>
