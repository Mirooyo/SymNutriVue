<template>
    <FoodMenu v-if="showFoodMenu" :selectedMeal="selectedMeal" @closeMenu="closeFoodMenu"/>
    <section class="meal-content"> 
        <p class="meal-title">Qu'avez vous mangé aujourd'hui ? <span v-show="isConnected"> {{ name }}</span></p>
        <div class="meal-container">
            <div class="meal-item">
                <div class="meal-item-icon">
                    <CircularProgress :progress="mealsCal[0].currentCalory" :maxProgress="mealsCal[0].maxCalory" />
                    <img src="../../public/meals-icons/chocolate.png" alt="" class="meal-item-img">
                </div>
                <div class="meal-item-info">
                    <p class="meal-item-title">Petit-déjeuner</p>
                    <p><span>{{ mealsCal[0].currentCalory }} kcal / {{ mealsCal[0].maxCalory }} kcal</span></p>
                </div>
                
                <div class="meal-content">
                    <button id="meal-1" @click="showFoodMenuWithMeal(1)">+</button>
                </div>
            </div>
            <div class="meal-item">
                <div class="meal-item-icon">
                    <img src="../../public/meals-icons/salad.png" alt="" class="meal-item-img">
                </div>
                <div class="meal-item-info">
                    <p class="meal-item-title">Déjeuner</p>
                    <p><span>{{ mealsCal[1].currentCalory }} kcal / {{ mealsCal[1].maxCalory }} kcal</span></p>
                </div>
                
                <div class="meal-content">
                    <button id="meal-1" @click="showFoodMenuWithMeal(2)">+</button>
                </div>
            </div>
            <div class="meal-item">
                <div class="meal-item-icon">
                    <img src="../../public/meals-icons/soup.png" alt="" class="meal-item-img">
                </div>
                <div class="meal-item-info">
                    <p class="meal-item-title">Diner</p>
                    <p><span>{{ mealsCal[2].currentCalory }} kcal / {{ mealsCal[2].maxCalory }} kcal</span></p>
                </div>
                
                <div class="meal-content">
                    <div v-for="meal in meals" :key="meal.id">{{ meal.name }}</div>
                </div>
            </div>
        </div>
    </section>
</template>
<script setup>
import CircularProgress from '../components/CircularProgress.vue';
import { ref } from 'vue';
import FoodMenu from '../components/FoodMenu.vue';

const name = ref("Badyss");
const mealsCal = ref(
    [{id: 1, currentCalory: 174, maxCalory: 286}, {id: 2, currentCalory: 163, maxCalory: 690}, {id: 3, currentCalory: 320, maxCalory: 350}]
);

const showFoodMenu = ref(false);
const selectedMeal = ref(2);

const showFoodMenuWithMeal = (mealId) => {
    selectedMeal.value = mealId;
    showFoodMenu.value = true;
}

const closeFoodMenu = () => {
    showFoodMenu.value = false;
}
</script>
<style>
    .meal-content {
        grid-column: 4 / 9;
        font-family: Poppins;
    }

    .meal-item {
        width: 300px;
        display: flex;
        flex-direction: row;
        align-items: center;
        justify-content: space-between;

    }

    .meal-item-title {
        font-weight: bold;
    }
    .meal-item-icon {
        margin-left: 30px;
        position: relative;
    }
    .circular-progress {
        position: absolute;
        width: 70px;
        top: -30px;
        left: -14px;
    }
    .meal-item-img {
        width: 40px;
    }
    .meal-container {
        height: 500px;
        background: #F0F7F3;
    }
    .meal-title {
        font-family: Poppins;
        font-weight: bold;
    }
</style>