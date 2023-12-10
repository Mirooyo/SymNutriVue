<script>
import { ref } from 'vue';
import FoodMenu from '../components/FoodMenu.vue';
import MealItem from '../components/MealItem.vue';
import UserInfo from './UserInfo.vue';
const BearerToken = "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJpYXQiOjE3MDIxMDg3NTEsImV4cCI6MTcwMjE5NTE1MSwicm9sZXMiOlsiUk9MRV9VU0VSIl0sInVzZXJuYW1lIjoiYmFkdHJ1YzEwQGdtYWlsLmNvbSJ9.WAkCHSd-grwVMCxw9wtC9IxZqZK9cfhk2TZsUVmtH4QQKKrYp2AXNRAkHambAsYGgCeJ8g9wGmU0LQHI7LLlVTWNiirU4wSkPW7pzeU4CaLEv5tbfARO1_JAUT_7rosRjDnrRI1j1tOhAhV1SfUbIMNxPOXWKygNADL3JVV_LHoAN1p9sHzcWS8Xdx9_yXDe_IXlqbzvwHNKA-zbGf6kurV4c6hVYeRfsVDiXcntKFP0KmSpiCGH5_lwzr6CYDKGXn_PJLay9TJHol4qtbBLaJsEG0N_p8UGnzOf1zs-F69Y1TuunxvVyxT55vMPrhOO-mNefkXcAzjIqLi2TkCVT9A5jOCafx6hGrGa_EQb9Lu7YVy8Z1BgyccQ-xYez4sJN0fNqKLj0JKhCHRrfqt8fFsFXXF2fjf5isc1h-_rUdrIUnxhlAIRzk9XkWcb3gyBRcTDfVlr30k99yQw3jDVDgghVjr9lKiDleOZODKNXfpoGJPL_tBb4pq_FwiTrazT8re0hEjOIf19sFwNRNmPehiD_5CfzFgSqwHYsHtl-q51wJHP2Eed2prbP205mfI2ziuODRvml5GcEpUKMEN_hsY55tM9FIKTJit0qTGrO0bg-DCQdUgFH9TzJR0PzTxfh3RUBD31MrimlQ0tsr9GLOK9Xr1yn_7GRm0L-aHShPw";

export default{
    name: "Meals",
    components: {
        FoodMenu,
        MealItem,
        UserInfo
    },
    setup(){
        let currentMealId = ref(null);
        let showFoodMenu = ref(false);
        const handleMealClicked = (mealId) => {
            currentMealId.value = mealId;
        }

        const handleFoodMenu = (active) => {
            showFoodMenu.value = active;
        }

        const handleFoodDisplay = (active) => {
            showFoodMenu.value = active;
        }

    return {
            currentMealId,
            handleMealClicked,
            showFoodMenu, 
            handleFoodMenu,
            handleFoodDisplay
        }
    }, 
    
}

</script>
<template>
    <UserInfo/>
    <FoodMenu :mealId="currentMealId" :key="currentMealId" v-if="showFoodMenu" :showFoodMenu="showFoodMenu" @mask-menu="handleFoodDisplay"/>
    <section class="meal-content"> 
        <p class="meal-title">Qu'avez vous mangé aujourd'hui ? <span></span></p>
        <div class="meal-container">
            <MealItem @meal-clicked="handleMealClicked" @food-menu="handleFoodMenu" :foodMenu="showFoodMenu"/>
        </div>
    </section>
</template>

<style>
    .meal-content {
        grid-column:  5 / 9;
        font-family: Poppins;
    }

    .meal-item {
        width: 300px;
        display: flex;
        border-bottom: 1px solid var(--primary-color);
        padding-bottom: 20px;
        flex-direction: row;
        align-items: center;
        justify-content: space-between;

    }

    .meal-item-title {
        font-weight: bold;
    }
    .meal-item-icon {
        position: relative;
        display: flex;
        flex-direction: row;
        align-items: center;
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

    .meal-item-info {
        margin-left: 10px;
    }
    .meal-container {
        height: fit-content;
        padding: 30px 20px;
        border-radius: 20px;
        width: fit-content;
        background: var(--background-color);
        color: var(--text-color);
    }
    .meal-title {
        font-family: Poppins;
        font-weight: bold;
        color: #F1E5E7;
    }
</style>