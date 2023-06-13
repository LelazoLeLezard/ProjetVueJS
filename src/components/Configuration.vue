<template>
    <div id="global">
        <div id="config">
            <Select id="livreDispo" label="Livre Disponible :" v-bind:values="livreDispo"></Select>
            <Select id="livreAjou" label="Livre Ajoutés :" v-bind:values="livreAjou"></Select>
        </div>
        <div id="button">
            <button @click="ajouter">Ajouter</button>
            <button @click="suprimer">Supprimer</button>
        </div>
    </div>
</template>

<script>
import { sortTable } from '@/assets/data.min.js'
import Select from './Select.vue';

export default {
  name: "Configuration",
components: {
    Select,
    },
    data(){
        return{
            livreDispo : this.initLivreDispo(),
            livreAjou : [],
            livresListe : [],
        }
    },
    getLivreListe(){
        return this.livresListe;
    },
methods: {
    getLivreD() {
        let livreD = [];
        sortTable.forEach(sort => {
            if (!livreD.includes(sort[0]))
                livreD.push(sort[0]);
        });
        return livreD;
    },
    initLivreDispo(){
        let livreD = JSON.parse(localStorage.getItem("livreDispo"));
        if (!livreD) {
            livreD = this.getLivreD();
            localStorage.setItem("livreDispo", JSON.stringify(livreD));
        }
        return livreD;
    },
    ajouter(){
        let livreD = document.getElementById("livreDispo");
        if(!this.livreAjou.includes(livreD.value)){
            sortTable.forEach(livre => {
                if(livre[0] == livreD.value){
                    this.livresListe.push(livre);
                }
            });
            this.livreAjou.push(livreD.value);
            this.livreDispo.splice(this.livreDispo.indexOf(livreD.value), 1);
        }
    },
    suprimer(){
        let livreA = document.getElementById("livreAjou");
        if(!this.livreDispo.includes(livreA.value)){
            sortTable.forEach(livre => {
                if(livre[0] == livreA.value){
                    if(sortTable.indexOf(livre) == 0){
                        this.livresListe.splice(0, 1);
                    }else{
                        this.livresListe.splice(sortTable.indexOf(livre),1);
                    }
                }
            });
            this.livreDispo.push(livreA.value);
            this.livreAjou.splice(this.livreAjou.indexOf(livreA.value), 1);
        }
    },
},
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