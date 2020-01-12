<template>
<div>
  <h1>Detail du restaurant</h1>
  <h3>Identifiant du restaurant :{{id}}</h3>
  <h3>Nom du restaurant : {{name}}</h3>
  <h3>Type de cuisine : {{cuisine}}</h3>
  <h3>situé a : {{borough}}</h3>
  <h3>adresse : {{building}}</h3>

  
  </div>
</template>

<script>
export default {
  name: "RestaurantDetail",
  props: {},
  computed: { // computed data, permet de définir des data "calculées"
      id() {
        // on y accèdera par {{id}} dans le template, et par this.id
        // dans le code
      return this.$route.params.id
      
    },
    // pour le nom de la restau
    name() {
      return this.$route.params.name
      
    },
    // pour la cuisine 
    cuisine() {
      return this.$route.params.cuisine
    },
    borough() {
      return this.$route.params.borough
    }
  },
      building() {
      return this.$route.params.address.building
    },

  data: function() {
    return {
      
    };
  },
  mounted() {
    console.log("AVANT AFFICHAGE !");
    console.log("On va chercher les détails du restaurant id = " + this.$route.params.id)
    console.log("On va chercher les détails du restaurant nom = " + this.$route.params.name)
    console.log("On va chercher les détails du restaurant cuisine = " + this.$route.params.cuisine)
    console.log("ID = " + this.id);
    console.log("ID = " + this.name);
    console.log("Cuisine = " + this.cuisine);
  },
  methods: {
    getDataFromServer() {
      // ici on fait un fetch pour récupérer le détail du restaurant

    },
    
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
