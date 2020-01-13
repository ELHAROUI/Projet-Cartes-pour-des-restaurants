<template>
  <div>
    <div class="border1">
      <p>
        <label>Rechercher par nom:</label>
        <input
          class="form-control"
          type="text"
          v-model="nomRecherche"
          v-on:input="getDataFromServer()"
        />
      </p>

      <p>
        <label>Nombre de restaurants par page :</label>
        <input
          class="custom-range form-control"
          type="range"
          min="2"
          max="100"
          value="10"
          v-on:input="getDataFromServer()"
          v-model="pagesize"
        />
        {{pagesize}}
      </p>
    </div>

    <br />
    <div class="border1">
      <form id="formulaireAjout" v-on:submit="postRequest(event);">
         <legend>Ajout d'un restaurant</legend>
        <div class="form-group">
          <label>Nom :</label>
          <input required class="form-control" name="nom" id="nom" type="text" v-model="name" />
        </div>

        <div class="form-group">
          <label>Cuisine :</label>
          <input
            required
            class="form-control"
            name="cuisine"
            id="cuisine"
            type="text"
            v-model="cuisine"
          />
          <input type="hidden" name="ids" v-model="ids" id="ids" />
        </div>

        <div class="form-group">
          <button class="form-control btn btn-success">Ajouter</button>
        </div>
      </form>
    </div>
    <div class="border2">

      <!-- Formulaire modfification -->
      <form id="formulaireModification" onsubmit="putRequest(event);">
        <legend>Modification d'un restaurant</legend>

        <div class="form-group">
          <label>Id :</label>
          <input
            required
            class="form-control"
            name="_id"
            type="text"
            placeholder="Id du restaurant à modifier"
            v-model="id"
          />
        </div>

        <div class="form-group">
          <label>Nom:</label>
          <input
            required
            class="form-control"
            name="nom"
            type="text"
            placeholder="Michel's restaurant"
            v-model="name"
          />
        </div>

        <div class="form-group">
          <label>Cuisine :</label>
          <input
            required
            class="form-control"
            name="cuisine"
            type="text"
            placeholder="Cuisine française"
            v-model="cuisine"
          />
        </div>
        <div class="form-group">
          <button class="form-control btn btn-success">modifier ce restaurant</button>
        </div>
      </form>

      <h1>Nombre de restaurants : {{nbRestaurants}}</h1>
      <button class="btn btn-warning" v-on:click="premierePage()">Première page</button>
      <button class="btn btn-info" v-on:click="pagePrecedente()" v-bind:disabled="page==0">Précédent</button>
      <button
        class="btn btn-info"
        v-on:click="pageSuivante()"
        :disabled="page == nbPagesDeResultats"
      >Suivant</button>
      <button class="btn btn-warning" v-on:click="dernierePage()">Dernière Page</button>
      <br />
      <br />
    </div>

    <H1>TABLE VUE-MATERIAL</H1>
    <md-table v-model="restaurants" md-sort="name" md-sort-order="asc" md-card md-fixed-header>
      <md-table-toolbar>
        <div class="md-toolbar-section-start">
          <h1 class="md-title">Trier par nom de restaurant</h1>
        </div>

        <md-field md-clearable class="md-toolbar-section-end">
          <md-input
            placeholder="Search by name..."
            v-model="nomRecherche"
            @input="getDataFromServer()"
          />
        </md-field>
      </md-table-toolbar>

      <md-table-empty-state
        md-label="No users found"
        :md-description="`No user found for this '${nomRecherche}' query. Try a different search term or create a new user.`"
      ></md-table-empty-state>
      <md-table-row class="test" slot="md-table-row" slot-scope="{ item }">
        <md-table-cell md-label="Name" md-sort-by="name">{{ item.name }}</md-table-cell>
        <md-table-cell md-label="location" md-sort-by="borough">{{ item.borough }}</md-table-cell>
        <md-table-cell md-label="Cuisine" md-sort-by="cuisine">{{ item.cuisine }}</md-table-cell>
        <md-table-cell md-label="Details">
          <router-link
            :to="'restaurant/'+item._id+'/'+item.name+'/'+item.cuisine+'/'+item.borough+'/'+item.grades[0]"
          >Details</router-link>
        </md-table-cell>
        <md-table-cell md-label="Supprimer">
          <button
            class="btn btn-danger"
            title="Supprimer"
            v-on:click="supprimerRestaurant(item._id)"
            href
          >Supprimer</button>
        </md-table-cell>
      </md-table-row>
    </md-table>
  </div>
</template>

<script>
export default {
  name: "Restaurants",
  props: {},
  data: function() {
    return {
      restaurants: [],
      nbRestaurants: 0,
      nom: "",
      cuisine: "",
      page: 0,
      pagesize: 10,
      nomRecherche: "",
      nbPagesDeResultats: 0,
      apiURL: "http://localhost:8080/api/restaurants"
    };
  },
  mounted() {
    console.log("AVANT AFFICHAGE !");
    this.getDataFromServer();
  },
  methods: {
    getDataFromServer() {
      // ici on fait un fetch pour récupérer des
      // restaurants sur le serveur.
      let url =
        this.apiURL +
        "?page=" +
        this.page +
        "&pagesize=" +
        this.pagesize +
        "&name=" +
        this.nomRecherche;

      fetch(url)
        .then(reponseJSON => {
          return reponseJSON.json();
        })
        .then(reponseJS => {
          // ici on a la réponse sous la forme
          // d'un objet JS
          this.restaurants = reponseJS.data;
          this.nbRestaurants = reponseJS.count;
          this.nbPagesDeResultats = Math.floor(
            this.nbRestaurants / this.pagesize
          );
        });
    },
    supprimerRestaurant(index) {
      this.restaurants.splice(index, 1);
    },

    getColor(index) {
      return index % 2 ? "lightBlue" : "pink";
    },
    pageSuivante() {
      console.log("Page suivante");
      this.page++;
      this.getDataFromServer();
    },
    pagePrecedente() {
      console.log("Page precedente");
      this.page--;
      this.getDataFromServer();
    },

    /* Premiere page */
    premierePage() {
      this.page = 0;
      this.getDataFromServer();
    },

    /* Derniere page*/
    dernierePage() {
      this.page =
        this.nbRestaurants % this.pagesize > 0
          ? parseInt(this.nbRestaurants / this.pagesize, 10)
          : parseInt(this.nbRestaurants / this.pagesize, 10);
      this.getDataFromServer();
    },

    /*Pour l'ajout */
    postRequest(event) {
      event.preventDefault();
      let form = event.target;
      let donneesFormulaire = new FormData(form);

      let url = "/api/restaurants";

      fetch(url, {
        method: "POST",
        body: donneesFormulaire
      })
        .then(function(responseJSON) {
          responseJSON.json().then(function(res) {
            let div = document.querySelector("#reponsePOST");
            div.innerHTML = res.msg;
          });
        })
        .catch(function(err) {
          console.log(err);
        });
    },

    // REQUETES GET
    /*getRequest1() {
      let url = "/api/restaurants";

      fetch(url)
        .then(function(responseJSON) {
          responseJSON.json().then(function(res) {
            // Maintenant res est un vrai objet JavaScript
            afficheReponseGET(res);
          });
        })
        .catch(function(err) {
          console.log(err);
        });
    },*/
/*afficheReponsePUT(reponse) {
      let div = document.querySelector("#reponsePUT");
      div.innerHTML = reponse.msg;

      // On affiche le tableau à jour
      //getRequest1();
    },*/

    // REQUETES PUT
    putRequest(event) {
      event.preventDefault();

      let form = event.target;

      let donneesFormulaire = new FormData(event.target);

      let id = form._id.value;

      let url = "/api/restaurants/" + id;

      fetch(url, {
        method: "PUT",
        body: donneesFormulaire
      })
        .then(function(responseJSON) {
          responseJSON.json().then(function(res) {
            // Maintenant res est un vrai objet JavaScript
            let div = document.querySelector("#reponsePUT");
            div.innerHTML = res.msg;
          });
        })
        .catch(function(err) {
          console.log(err);
        });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.border1,
.border2 {
  width: 50%;
  margin: 0 auto;
}
.border1 {
  border-bottom: 2px solid red;
}

.border3 {
   float: left;
    clear: none;
}

input {
  width: 100%;
}
</style>
