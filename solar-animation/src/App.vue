<template>
  <div class="home">
    <router-view />
  </div>
</template>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}

.navbar-component {
  position: relative;
  //background-color: #f0f0f0;
  padding: 10px;
}
</style>

<script>
//import NavbarComponent from "@/components/NavBar.vue";

export default {
  // Define the component name
  name: "App",

  // Define the components property
  components: {
    //NavbarComponent,
  },
};
</script>
