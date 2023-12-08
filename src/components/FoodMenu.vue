<script setup>
import { ref } from "vue";
const searchName = ref('');
let products = ref([]);
const props = defineProps(["selectedMeal"]);
const pageSize = 7;
const currentPage = ref(1);

const searchProduct = (searchTerm) => {
  const apiUrl = `https://world.openfoodfacts.org/cgi/search.pl?search_terms=${searchTerm}&search_simple=1&action=process&json=1&page_size=${pageSize}&page=${currentPage.value}`;
  
  fetch(`${apiUrl}`)
    .then(response => {
      if (!response.ok) {
        throw new Error(`Erreur lors de la requête : ${response.status}`);
      }
      return response.json();
    })
    .then(result => {
      if (result.count > 0) {
       products.value = result.products;
       console.log(result.products[0].nutriments['energy-kcal']);
      } else {
        products.value = [];
        console.log('Aucun produit trouvé.');
      }
    })
    .catch(error => {
      console.error('Erreur lors de la requête :', error);
    });
}

const nextPage = () => {
  currentPage.value++;
  searchProduct(searchName.value);
}

const prevPage = () => {
  if (currentPage.value > 1) {
    currentPage.value--;
    searchProduct(searchName.value);
  }
}


const submitForm = () => {
  searchProduct(searchName.value);
}
</script>

<template>
    <div class="food-wrapper">
        <form @submit.prevent="submitForm">
            <label for="search" id="search-label">Rechercher un produit</label>
            <input type="text" name="search" id="search" v-model="searchName">
            <button type="submit">Rechercher</button>
        </form>
        <div class="food-container">
            <div class="food-result">
                <div v-for="product in products" :key="product.code" class="food-item">
                    <p>{{ product.product_name }}</p>
                    <p>Calories: {{ product.nutriments['energy-kcal'] }} {{ product.nutriments['energy-kcal_unit'] }}</p>
                </div>
            </div>
            <div class="pagination">
                <button @click="prevPage" :disabled="currentPage <= 1">Précédent</button>
                <span>Page {{ currentPage }}</span>
                <button @click="nextPage" :disabled="products.length < pageSize">Suivant</button>
            </div>
        </div>
    </div>
</template>

<style>
    .food-wrapper {
        width: 300px;
        display: flex;
        flex-direction: column;
        background: var(--background-color);
    }
    #search-label {
        font-family: Poppins;
        font-weight: bold;
    }

    #search {
        background: #F0F7F3;
        border: none;
        outline: none;
        font-family: Poppins;
        padding: 5px;
        border-radius: 30px;
    }
    .food-container {
        padding: 10px;
        background: var(--background-color);
    }
    .food-result {
        display: flex;
        flex-direction: column;
        gap: 20px;
    }
    .food-item {
        display: flex;
        flex-direction: column;
        width: 200px;
        padding: 5px 10px;
        border-radius: 10px;
        font-family: Poppins;
        background: #F0F7F3;
    }

    .food-item p {
        margin: 0;
    }
</style>

