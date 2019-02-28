<template>
  <div id="app" class="has-background-white-bis	is-overlay is-clipped is-clearfix">
    <Hero></Hero>
    <Main></Main>
    <Footer></Footer>
  </div>
</template>

<script>
import Main  from "./components/Main.vue";
import Hero from "./components/Hero.vue";
import Footer from "./components/Footer.vue";

export default {
  name: "app",
  components: {
    Main,
    Hero,
    Footer
  }
};
</script>

<style>
</style>