<script setup>
import { ref } from "vue";
const searchName = ref('');
const pageSize = 7;
let products = ref([]);
const currentPage = ref(1);
const props = defineProps(['mealId']);

const BearerToken = "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJpYXQiOjE3MDIxMDg3NTEsImV4cCI6MTcwMjE5NTE1MSwicm9sZXMiOlsiUk9MRV9VU0VSIl0sInVzZXJuYW1lIjoiYmFkdHJ1YzEwQGdtYWlsLmNvbSJ9.WAkCHSd-grwVMCxw9wtC9IxZqZK9cfhk2TZsUVmtH4QQKKrYp2AXNRAkHambAsYGgCeJ8g9wGmU0LQHI7LLlVTWNiirU4wSkPW7pzeU4CaLEv5tbfARO1_JAUT_7rosRjDnrRI1j1tOhAhV1SfUbIMNxPOXWKygNADL3JVV_LHoAN1p9sHzcWS8Xdx9_yXDe_IXlqbzvwHNKA-zbGf6kurV4c6hVYeRfsVDiXcntKFP0KmSpiCGH5_lwzr6CYDKGXn_PJLay9TJHol4qtbBLaJsEG0N_p8UGnzOf1zs-F69Y1TuunxvVyxT55vMPrhOO-mNefkXcAzjIqLi2TkCVT9A5jOCafx6hGrGa_EQb9Lu7YVy8Z1BgyccQ-xYez4sJN0fNqKLj0JKhCHRrfqt8fFsFXXF2fjf5isc1h-_rUdrIUnxhlAIRzk9XkWcb3gyBRcTDfVlr30k99yQw3jDVDgghVjr9lKiDleOZODKNXfpoGJPL_tBb4pq_FwiTrazT8re0hEjOIf19sFwNRNmPehiD_5CfzFgSqwHYsHtl-q51wJHP2Eed2prbP205mfI2ziuODRvml5GcEpUKMEN_hsY55tM9FIKTJit0qTGrO0bg-DCQdUgFH9TzJR0PzTxfh3RUBD31MrimlQ0tsr9GLOK9Xr1yn_7GRm0L-aHShPw";

// fetch la liste des produits avec OpenFoodFact
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
      } else {
        products.value = [];
        console.log('Aucun produit trouvé.');
      }
    })
    .catch(error => {
      console.error('Erreur lors de la requête :', error);
    });
}

// Pagination
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

const addToMeal = async (product) => {
  let productName = product.product_name;
  let calory = product.nutriments['energy-kcal'];
  let mealID = props.mealId;
  const url = 'http://127.0.0.1:8000/api/foods';
        const data = {
            name: productName,
            description: "test test test",
            calory: calory,
            meal_id: mealID
        };

        fetch(url, {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
            'Authorization': `Bearer ${BearerToken}`
        },
        body: JSON.stringify(data),
        })
        .then(response => response.json())
        .then(data => console.log('Réponse du serveur:', data))
        .catch(error => console.error('Erreur:', error));
}

const isMenuVisible = ref(true);

const maskMenu = () => {
  isMenuVisible.value = false;
}



</script>

<template>
    <div class="food-wrapper" v-show="isMenuVisible" >
        <button @click="$emit('mask-menu', false), maskMenu()" class="close__button">X</button>
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
                    <button @click="addToMeal(product)">Ajouter au repas</button>
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

<style scoped>
    .food-wrapper {
        z-index: 3;
        width: 300px;
        padding: 5px;
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
        height: fit-content;
        align-items: center;
        border-radius: 10px;
        box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px;
        border: 1px solid white;
        display: flex;
        flex-direction: column;
        background: var(--background-color);
    }

    .close__button {
      position: absolute;
      right: 10px;
    }


    #search-label {
        font-family: Poppins;
        font-weight: bold;
        color: var(--text-color);
    }

    button {
      font-family: Poppins;
      background: var(--primary-color);
      border: 1px solid white;
      border-radius: 5px;
      transition: all .2s ease;
      color: var(--text-color);
    }

    button:hover {
      background-color: rgba(255, 255, 255, .2);
      border: 1px solid var(--primary-color);
      cursor: pointer;
    }

    p {
      font-size: .9em;
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
        margin-bottom: 20px;
    }
    span {
      font-family: Poppins;
      font-weight: bold;
      margin: 0 10px;
      color: var(--text-color);
    }
    .food-item {
        display: flex;
        flex-direction: column;
        width: 200px;
        padding: 5px 10px;
        border-radius: 10px;
        font-family: Poppins;
        background: var(--secondary-color);
        color: var(--text-color);
    }

    .food-item p {
        margin: 0;
    }
</style>

