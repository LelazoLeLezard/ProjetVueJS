<template>
  <nav class="navbar navbar-dark bg-dark">
    <div class="container">
      <a class="nav-link" v-on:click="changeContent('Recherche')">Recherche</a>
      <a class="nav-link" v-on:click="changeContent('Configuration')">Configuration</a>
      <a class="nav-link" v-on:click="changeContent('Statistique')">Statistique</a>
    </div>
  </nav>
</template>

<script>
export default {
  name: "Navbar",
  methods: {
    changeContent (content) {
      this.$parent.content = content;
    }
  }
};
</script>

<style scoped>
a {
  color: red;
  font-size: 1.5em;
}

a:hover {
  cursor: pointer;
}
</style>
