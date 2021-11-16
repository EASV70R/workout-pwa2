<template>
  <div id="app">
    <NavBar @langChange="handleLangChange" />
    <router-view :lang="lang" />
    <Footer></Footer>
  </div>
</template>

<script>
import NavBar from "@/components/NavBar";
import Footer from "@/components/Footer";
export default {
  components: {
    NavBar,
    Footer
  },
  data() {
    return {
      lang: "DA"
    };
  },
  methods: {
    handleLangChange(lang) {
      this.lang = lang;
    }
  }
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Rubik:wght@400;500;700&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Ubuntu&display=swap");

:root {
  /* --main-bg-color: #343bd9; */
  /* --second-bg-color: #0c129c; */
  --main-bg-color: #21234b;
  --second-bg-color: #10153b;
  --main-text-color: rgb(0, 0, 0);
  --main-light-bg-color: #f5fafa;
  --second-light-bg-color: #acd1e9;
}

body {
  font-family: "Rubik", sans-serif;
}

.light-theme {
  background-color: var(--main-light-bg-color);
  color: var(--second-light-bg-color);
}
</style>
