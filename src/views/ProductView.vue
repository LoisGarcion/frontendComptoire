<template>
  <div>
    <h1>Les produits - page {{data.page}} / {{data.totalPages}}</h1>
    <table>
      <caption>Liste des catégories</caption>
      <tr>
        <th>Nom</th>
        <th>Prix</th>
        <th>Stock</th>
        <th>Commandés</th>
      </tr>
      <!-- Si le tableau des catégories n'est pas vide -->
      <tr v-for="produit in data.listeProduits" :key="produit.reference">
        <td>{{ produit.nom }}</td>
        <td>{{ produit.prixUnitaire }}</td>
        <td>{{ produit.unitesEnStock }}</td>
        <td>{{ produit.unitesCommandees }}</td>
      </tr>
    </table>
    <button @click="data.page = 0; chargeProduits()">Premiere page</button>
    <button @click="data.page = data.page - 1; chargeProduits()" :disabled="data.page === 0">Précédent</button>
    <button @click="data.page = data.page + 1; chargeProduits()" :disabled="data.page === data.totalPages">Suivant</button>
    <button @click="data.page = data.totalPages; chargeProduits()">Derniere page</button>
  </div>
</template>

<script>
export default {
  name: "ProductView"
}
</script>

<style scoped>
td,
th {
  border: 1px solid #ddd;
  padding: 8px;
}

table{
  width: 80%;
}


th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: #232623;
  color: rgb(255, 255, 255);
}
</style>

<script setup>
import { reactive, onMounted } from "vue";
import {doAjaxRequest } from "@/api";

let data = reactive({
  // La liste des catégories affichée sous forme de table
  listeProduits: [],
  page: 0,
  size:5,
  totalPages: 0
});
function chargeProduits() {
  // Appel à l'API pour avoir la liste des catégories
  // Trié par code, descendant
  // Verbe HTTP GET par défaut
  doAjaxRequest("https://springajax.herokuapp.com/api/produits?page=" + data.page + "&size=" + data.size)
      .then((json) => {
        data.listeProduits = json._embedded.produits;
        data.totalPages = json.page.totalPages -1;
      })
      .catch((error) => alert(error.message));
}

onMounted(chargeProduits);

</script>

