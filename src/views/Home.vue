<template>
  <div class="home">
    <div class="container">
      <div class="jumbotron">
        <h1 class="display-4">Hello, world!</h1>
        <p class="lead">This is a simple hero unit, a simple jumbotron-style component for calling extra attention to featured content or information.</p>
        <hr class="my-4">
        <p>It uses utility classes for typography and spacing to space content out within the larger container.</p>
        <a class="btn btn-primary btn-lg" href="#" role="button">Learn more</a>
      </div>

      <h1>Add new recipe</h1>
      <ul>
        <li v-for="error in errors" class="text-danger">{{error}}</li>
      </ul>
      Name: <input v-model="newRecipeName" type="text">
      Chef: <input v-model="newRecipeChef" type="text">
      Ingredients: <input v-model="newRecipeIngredients" type="text">
      Directions: <input v-model="newRecipeDirections" type="text">
      <button v-on:click="createRecipe()" class="btn btn-primary">Create</button>

      <div>
        <button v-on:click="sortAttribute = 'title'" class="btn btn-secondary">Sort by title</button>
        <button v-on:click="sortAttribute = 'chef'" class="btn btn-secondary">Sort by chef</button>
      </div>
      <h1>Search recipes</h1>
      <input type="text" v-model="searchFilter" list="names">
      <datalist id="names">
        <option v-for="recipe in recipes">{{ recipe.title }}</option>
      </datalist>
      <div class="row">
        <div v-for="recipe in orderBy(filterBy(recipes, searchFilter, 'title', 'ingredients'), sortAttribute)" class="col-md-4 mb-2">
          <div class="card">
            <img class="card-img-top" v-bind:src="recipe.image_url" alt="Card image cap">
            <div class="card-body">
              <h5 class="card-title">{{ recipe.title }}</h5>
              <p class="card-text">Chef: {{ recipe.chef }}</p>
              <p class="card-text">Ingredients: {{ recipe.ingredients }}</p>
              <a v-bind:href="`/#/recipes/${recipe.id}`" class="btn btn-primary">Go somewhere</a>

              <button v-on:click="setCurrentRecipe(recipe)" type="button" class="btn btn-primary" data-toggle="modal" data-target="#exampleModal">
                Launch demo modal
              </button>

            </div>
          </div>
        </div>
      </div>

      <div class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog" role="document">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title" id="exampleModalLabel">{{ currentRecipe.title }}</h5>
              <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
              </button>
            </div>
            <div class="modal-body">
              {{ currentRecipe.ingredients }}
            </div>
            <div class="modal-footer">
              <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
              <button type="button" class="btn btn-primary">Save changes</button>
            </div>
          </div>
        </div>
      </div>

    </div>
  </div>
</template>

<style>
</style>

<script>
var axios = require("axios");

export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      recipes: [],
      currentRecipe: {},
      newRecipeName: "",
      newRecipeChef: "",
      newRecipeIngredients: "",
      newRecipeDirections: "",
      searchFilter: "",
      sortAttribute: "title",
      errors: []
    };
  },
  created: function() {
    axios.get("http://localhost:3000/api/recipes").then(
      function(response) {
        console.log(response.data);
        this.recipes = response.data;
      }.bind(this)
    );
  },
  methods: {
    setCurrentRecipe: function(inputRecipe) {
      this.currentRecipe = inputRecipe;
    },
    createRecipe: function() {
      console.log("createRecipe");
      this.errors = [];
      var params = {
        input_title: this.newRecipeName,
        input_chef: this.newRecipeChef,
        input_ingredients: this.newRecipeIngredients,
        input_directions: this.newRecipeDirections
      };
      axios
        .post("http://localhost:3000/api/recipes", params)
        .then(
          function(response) {
            console.log(response);
            this.recipes.push(response.data);
            this.newRecipeName = "";
            this.newRecipeChef = "";
            this.newRecipeIngredients = "";
            this.newRecipeDirections = "";
          }.bind(this)
        )
        .catch(
          function(error) {
            console.log(error.response.data.errors);
            this.errors = error.response.data.errors;
          }.bind(this)
        );
    }
  },
  computed: {}
};
</script>
