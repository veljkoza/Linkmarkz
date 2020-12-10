<template>
  <div id="app">
    <div class="card-container">
      <h1 v-if="allLinks.length === 0">No saved links</h1>
      <link-card
        v-else
        v-for="link in allLinks"
        :key="link"
        :url="link"
        @refreshLinks="refreshLinks()"
      ></link-card>
    </div>
    <floating-btn @refreshLinks="refreshLinks()" />
  </div>
</template>

<script>
import "./_assets/style.scss";
import LinkCard from "./components/LinkCard";
import FloatingBtn from "./components/FloatingBtn.vue";
export default {
  name: "App",
  components: {
    LinkCard,
    FloatingBtn,
  },
  data() {
    return {
      allLinks: JSON.parse(localStorage.getItem("allLinks"))
        ? JSON.parse(localStorage.getItem("allLinks"))
        : [],
    };
  },
  methods: {
    refreshLinks() {
      this.allLinks = JSON.parse(localStorage.getItem("allLinks"));
      console.log(this.allLinks)
    },
  },
};
</script>

<style lang="scss" scoped>


</style>
