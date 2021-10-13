<template>  
  <div id="app">
    <Header />
    <Form :height="height" :inches="inches" :mass="mass" :system="system" />
    <Footer />
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Form from "./components/Form.vue";
import Footer from './components/Footer.vue';
export default {
  name: "App",
  components: {
    Header,
    Form,
    Footer
  },
  data() {
    return {
      height: null,
      inches: null,
      mass: null,
      system: "imperial"
    }
  },
}
</script>

<style lang="scss">
  @import "./scss/variables.scss";
  @import "./scss/mixins.scss";
  body {
    margin: 0;
    padding: 0;
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    background-color:$background;
    color: $text;
    #app {
      max-width:500px;
      margin:10px auto;
      padding: 20px;
    }
  }
  // X-Small devices (portrait phones, less than 576px)
  @media (max-width: 575.98px) {
    body #app {
      max-width:400px;
    }
  }
</style>