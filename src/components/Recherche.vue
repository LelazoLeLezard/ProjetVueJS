<template>
  <div id="global">
    <form id="config">
      <div>
        <Select id="ecoles" label="École :" v-bind:values="ecoles"></Select>
        <input type="checkbox" value="ecole" id="checkboxEcoles" v-model="checked">
        <label for="checkbox">prendre en compte ?</label>
      </div>
      <div>
        <Select id="branches" label="Branche :" v-bind:values="branche"></Select>
        <input type="checkbox" value="branche" id="checkboxBranche" v-model="checked">
        <label for="checkbox">prendre en compte ?</label>
      </div>
      <div>
        <Select id="classe" label="Classe & Domaine :" v-bind:values="classe"></Select>
        <input type="checkbox" value="class" id="checkboxClass" v-model="checked">
        <label for="checkbox">prendre en compte ?</label>
      </div>
      <div>
        <Select id="lv" label="Niveau :" v-bind:values="lv"></Select>
        <input type="checkbox" value="lv" id="checkboxLv" v-model="checked">
        <label for="checkbox">prendre en compte ?</label>
      </div>
    </form>
    <div id="button">
      <button @click="afficherListeLivre">Rechercher</button>
    </div>
    <div id="affichage">
    </div>
    
  </div>
</template>

<script>
import { sortTable } from '@/assets/data.min.js'
import Select from "./Select.vue";
import Configuration from './Configuration.vue';

export default {
  name: "Recherche",
  components: {
    Select,
},
  data() {
    return {
      ecoles : this.initSchools(),
      branche : this.initBranche(),
      classe : this.initClasse(),
      lv : this.initLv(),
      livreDispo : this.initLivreDispo(),
      checked : [],
    }
  },
  methods: {
    getSchools() {
      let schools = []; // tableau qui contient les différentes écoles
      sortTable.forEach(sort => {
        if (!schools.includes(sort[2])) // évite les doublons
          schools.push(sort[2]);
      });
      return schools;
    },
    initSchools() {
      let schools = JSON.parse(localStorage.getItem("ecoles"));
      if (!schools) {
        schools = this.getSchools();
        localStorage.setItem("ecoles", JSON.stringify(schools));
      }
      return schools;
    },
    initBranche(){
      let branches = JSON.parse(localStorage.getItem("branche"));
      if (!branches) {
        branches = this.getBranches();
        localStorage.setItem("branche", JSON.stringify(branches));
      }
      return branches;
    },
    getBranches() {
      let branches = []; // tableau qui contient les différentes écoles
      sortTable.forEach(sort => {
        sort[3].forEach(elem => {
          if (!branches.includes(elem)){ // évite les doublons
            branches.push(elem);
          }
        });
      });
      return branches;
    },
    initClasse(){
      let classees = JSON.parse(localStorage.getItem("classe"));
      if (!classees) {
        classees = this.getClasse();
        localStorage.setItem("classe", JSON.stringify(classees));
      }
      return classees;
    },
    getClasse() {
      let classees = []; // tableau qui contient les différentes écoles
      sortTable.forEach(sort => {
        sort[4].forEach(elem => {
          if (!classees.includes(elem[0])){ // évite les doublons
            classees.push(elem[0]);
          }
        });
      });
      return classees;
    },
    initLv(){
      let lvs = JSON.parse(localStorage.getItem("lv"));
      if (!lvs) {
        lvs = this.getLv();
        localStorage.setItem("lv", JSON.stringify(lvs));
      }
      return lvs;
    },
    getLv() {
      let lvs = []; // tableau qui contient les différentes écoles
      for(let i=0;i<10;i++){
        lvs.push(i);
      }
      return lvs;
    },
    initLivreDispo() {
      let livre = []
      return livre;
    },
    chercher(){
      let livres = this.$parent.getConfiguration().getLivreListe();
      console.log(livres);
      let livreDi = [];
      let check = this.$data.checked;
      let testAjout;
      sortTable.forEach(elem => {
        testAjout = true;
        if(check.includes("ecole")){
          if(!(elem[2] === document.getElementById("ecoles").value)){
            if(!(livreDi.includes(elem))){
              testAjout = false;
            }
          }
        }
        if(check.includes("branche")){
          if(!(elem[3].includes(document.getElementById("branches").value))){
            if(!(livreDi.includes(elem))){
              testAjout = false;
            }
          }
        }
        if(check.includes("class")){
          elem[4].forEach(classe => {
            if(!(classe[0] == document.getElementById("classe").value)){
              if(!(livreDi.includes(elem))){
                testAjout = false;
              }
            }
          });
        }
        if(check.includes("lv")){
          elem[4].forEach(classe => {
            if((classe[1] == document.getElementById("lv").value)){
              if(!(livreDi.includes(elem))){
                testAjout = false;
              }
            }
          });
        }
        if(testAjout){
          livreDi.push(elem);
        }
      });
      this.$data.livreDispo = livreDi;
      console.log(this.$data.livreDispo);
      return livreDi;
    },
    butSuppr(){
      this.$data.livreDispo = [];
      document.getElementsByClassName("p").forEach(elem => {
        document.getElementById("affichage").removeChild(elem);
      });
    },
    afficherListeLivre(){
      let livreDi = this.chercher();

      let div = document.getElementById("affichage");

      if(div.hasChildNodes()){
        div.childNodes.forEach(child => {
          div.removeChild(child);
        });
      }

      livreDi.forEach(livre => {
        let panel = document.createElement("div");
        panel.setAttribute("class","panel");

        let panelTitle = document.createElement("div");
        panelTitle.setAttribute("class", "panel-heading");
        panel.appendChild(panelTitle);

        let title = document.createElement("h2");
        title.setAttribute("class","panel-title");
        title.textContent = livre[1];
        panelTitle.appendChild(title);

        let panelContent = document.createElement("div");
        panelContent.setAttribute("class","panel-content");
        panel.appendChild(panelContent);

        let br

        let livreType = document.createElement("b");
        livreType.textContent = "Livre : ";
        panelContent.appendChild(livreType);

        let elivreType = document.createElement("I");
        elivreType.textContent = livre[0];
        panelContent.appendChild(elivreType);

        br = document.createElement("br");
        panelContent.appendChild(br);

        let ecole = document.createElement("b");
        ecole.textContent = "Ecole : ";
        panelContent.appendChild(ecole);

        let eecole = document.createElement("I");
        eecole.textContent = livre[2];
        panelContent.appendChild(eecole);

        br = document.createElement("br");
        panelContent.appendChild(br);

        let branche = document.createElement("b");
        branche.textContent = "Branches : ";
        panelContent.appendChild(branche);

        let ebranche = document.createElement("I");
        ebranche.textContent = livre[3];
        panelContent.appendChild(ebranche);

        br = document.createElement("br");
        panelContent.appendChild(br);

        let classe = document.createElement("b");
        classe.textContent = "Classes : ";
        panelContent.appendChild(classe);

        let eclasse = document.createElement("I");
        eclasse.textContent = livre[4];
        panelContent.appendChild(eclasse);

        br = document.createElement("br");
        panelContent.appendChild(br);

        let composantes = document.createElement("b");
        composantes.textContent = "Composantes : ";
        panelContent.appendChild(composantes);

        let ecomposantes = document.createElement("I");
        ecomposantes.textContent = livre[5];
        panelContent.appendChild(ecomposantes);

        br = document.createElement("br");
        panelContent.appendChild(br);

        let temps = document.createElement("b");
        temps.textContent = "Temps d'incantation : ";
        panelContent.appendChild(temps);

        let etemps = document.createElement("I");
        etemps.textContent = livre[6];
        panelContent.appendChild(etemps);

        br = document.createElement("br");
        panelContent.appendChild(br);

        let portee = document.createElement("b");
        portee.textContent = "Portée : ";
        panelContent.appendChild(portee);

        let eportee = document.createElement("I");
        eportee.textContent = livre[7];
        panelContent.appendChild(eportee);

        br = document.createElement("br");
        panelContent.appendChild(br);

        let cible = document.createElement("b");
        cible.textContent = "Cible : ";
        panelContent.appendChild(cible);

        let ecible = document.createElement("I");
        ecible.textContent = livre[8];
        panelContent.appendChild(ecible);

        br = document.createElement("br");
        panelContent.appendChild(br);

        let duree = document.createElement("b");
        duree.textContent = "Durée : ";
        panelContent.appendChild(duree);

        let eduree = document.createElement("I");
        eduree.textContent = livre[9];
        panelContent.appendChild(eduree);

        br = document.createElement("br");
        panelContent.appendChild(br);

        let jet = document.createElement("b");
        jet.textContent = "Jet de sauvegarde : ";
        panelContent.appendChild(jet);

        let ejet = document.createElement("I");
        ejet.textContent = livre[10];
        panelContent.appendChild(ejet);

        br = document.createElement("br");
        panelContent.appendChild(br);

        let resistance = document.createElement("b");
        resistance.textContent = "Résistance à la magie : ";
        panelContent.appendChild(resistance);

        let eresistance = document.createElement("I");
        eresistance.textContent = livre[11];
        panelContent.appendChild(eresistance);

        br = document.createElement("br");
        panelContent.appendChild(br);

        let description = document.createElement("b");
        description.textContent = "Description : ";
        panelContent.appendChild(description);

        let edescription = document.createElement("I");
        edescription.textContent = livre[12];
        panelContent.appendChild(edescription);

        br = document.createElement("br");
        panelContent.appendChild(br);

        let necessaire = document.createElement("b");
        necessaire.textContent = "Nécessaire : ";
        panelContent.appendChild(necessaire);

        let enecessaire = document.createElement("I");
        enecessaire.textContent = livre[13];
        panelContent.appendChild(enecessaire);

        div.appendChild(panel);

      });
    }
  }
};
</script>

<style scoped>
#config {
  padding: 4em;
  font-size: 1.2em;
  color: darkred;
}

#button{
  padding: 1em;
}
button{
  margin: 1em;
}
#affichage{
  padding: 2em;
  margin: 1em;
}
</style>
