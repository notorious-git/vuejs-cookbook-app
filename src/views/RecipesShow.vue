<template>
  <div class="home">
    <div class="container">
      <h1>Recipe info</h1>
      {{ recipe }}
      <a href="/#/">Back to all recipes</a>
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
      recipe: {}
    };
  },
  created: function() {
    axios.get("http://localhost:3000/api/recipes/3").then(response => {
      this.recipe = response.data;
    });
  },
  methods: {},
  computed: {}
};
</script>
