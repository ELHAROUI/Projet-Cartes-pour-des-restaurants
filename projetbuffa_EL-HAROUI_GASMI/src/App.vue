<template>
  <div id="app">
  <nav class="navbar navbar-light bg-light">
  <form class="form-inline">
    <button class="btn btn-outline-success" type="button"><router-link to="/">Home</router-link></button>
    <button class="btn btn-outline-success" type="button"><router-link to="/restaurants">Restaurants</router-link></button>
      </form>
</nav>
     <router-view></router-view>
  </div>
</template>

<script>

export default {
  name: "app",
  components: {
  },
  data: function() {
    return {
      nom: "buffa"
    };
  },
  mounted()  {
    console.log("avant affichage");
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.form-inline {
  width: 15%;
  margin: 0 auto;
}

</style>
