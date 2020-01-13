<template>
  <div>
    <br />
    <h1>Detail du restaurant</h1>
    <br />
    <table class="table">
      <thead>
        <tr>
          <th scope="col">Identifiant du restaurant</th>
          <th scope="col">Nom du restaurant</th>
          <th scope="col">Type de cuisine</th>
          <th scope="col">Ville</th>
          <th scope="col">Quartier</th>
          <th scope="col">longitude</th>
          <th scope="col">latitude</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>{{id}}</td>
          <td>{{name}}</td>
          <td>{{cuisine}}</td>
          <td>{{borough}}</td>
          <td>{{street}}</td>
          <td>{{longitude}}</td>
          <td>{{latitude}}</td>
        </tr>
      </tbody>
    </table>
    <br />
    <h1>Avis client</h1>
    <table class="table">
      <thead>
        <tr>
          <th scope="col">Avis client</th>
          <th scope="col">Date</th>
          <th scope="col">grade</th>
          <th scope="col">score</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th scope="row">1</th>
          <td>{{date0}}</td>
          <td>{{grade0}}</td>
          <td>{{score0}}</td>
        </tr>
        <tr>
          <th scope="row">2</th>
          <td>{{date1}}</td>
          <td>{{grade1}}</td>
          <td>{{score1}}</td>
        </tr>
      </tbody>
    </table>
    <br />

    <div>
      <img src="@/assets/images-restaus/image1.jpg" />">
    </div>
  </div>
</template>

<script>
export default {
  name: "RestaurantDetail",
  props: {},
  computed: {
    // computed data, permet de définir des data "calculées"
    id() {
      // on y accèdera par {{id}} dans le template, et par this.id
      // dans le code
      return this.$route.params.id;
    },
    // pour le nom de la restau
    name() {
      return this.$route.params.name;
    },
    date0() {
      return this.$route.params.date0;
    },
    grade0() {
      return this.$route.params.grade0;
    },
    score0() {
      return this.$route.params.score0;
    },
    date1() {
      return this.$route.params.date1;
    },
    grade1() {
      return this.$route.params.grade1;
    },
    score1() {
      return this.$route.params.score1;
    },
    longitude() {
      return this.$route.params.longitude;
    },
    latitude() {
      return this.$route.params.latitude;
    },
    street() {
      return this.$route.params.street;
    },
    // pour la cuisine
    cuisine() {
      return this.$route.params.cuisine;
    },
    borough() {
      return this.$route.params.borough;
    }
  },

  data: function() {
    return {};
  },
  mounted() {
    console.log("AVANT AFFICHAGE !");
    console.log(
      "On va chercher les détails du restaurant id = " + this.$route.params.id
    );
    console.log(
      "On va chercher les détails du restaurant nom = " +
        this.$route.params.name
    );
    console.log(
      "On va chercher les détails du restaurant cuisine = " +
        this.$route.params.cuisine
    );
    console.log("ID = " + this.id);
    console.log("ID = " + this.name);
    console.log("Cuisine = " + this.cuisine);
  },
  methods: {
    getDataFromServer() {
      // ici on fait un fetch pour récupérer le détail du restaurant
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.table {
  width: 70%;
  margin: 0 auto;
  border-bottom: 1px solid red;
}
</style>
