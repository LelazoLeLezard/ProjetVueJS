<template>
  <div id="content">
    <Recherche v-show="getContent() === 'Recherche'"></Recherche>
    <Statistique v-show="getContent() === 'Statistique'"></Statistique>
    <Configuration v-show="getContent() === 'Configuration'"></Configuration>
  </div>
</template>

<script>
import Recherche from "./Recherche.vue";
import Statistique from "./Statistique.vue";
import Configuration from "./Configuration.vue";


export default {
  name: "Content",
  components: {
    Recherche,
    Statistique,
    Configuration
},
  methods : {
    getContent() {
      return this.$parent.content;
    },
    getConfiguration(){
      return Configuration
    }
  }
};
</script>

<style scoped>
#content {
  flex: 1 1 auto;
  background-color: slategrey;
}
</style>
