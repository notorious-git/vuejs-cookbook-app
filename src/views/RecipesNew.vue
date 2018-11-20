<template>
  <div class="home">
    <div class="container">

      <h1>Add new recipe</h1>
      <ul>
        <li v-for="error in errors" class="text-danger">{{error}}</li>
      </ul>
      Name: <input v-model="newRecipeName" type="text">
      Chef: <input v-model="newRecipeChef" type="text">
      Ingredients: <input v-model="newRecipeIngredients" type="text">
      Directions: <input v-model="newRecipeDirections" type="text">
      <button v-on:click="createRecipe()" class="btn btn-primary">Create</button>

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
      newRecipeName: "",
      newRecipeChef: "",
      newRecipeIngredients: "",
      newRecipeDirections: "",
      errors: []
    };
  },
  created: function() {},
  methods: {
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
            this.newRecipeName = "";
            this.newRecipeChef = "";
            this.newRecipeIngredients = "";
            this.newRecipeDirections = "";
            this.$router.push("/");
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
