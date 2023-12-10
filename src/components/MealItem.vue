<template>
    <div class="meal-item" :data-id="meal.id" v-for="meal in data_meal" :key="meal.id">
        
        <div class="meal-item-icon">
            <img :src="getMealIcon(meal.nom)" alt="" class="meal-item-img">
            <div class="meal-item-info" >
            <p class="meal-item-title" v-if="meal.nom === 1">Petit-Déjeuner</p>
            <p class="meal-item-title" v-else-if="meal.nom === 2">Déjeuner</p>
            <p class="meal-item-title" v-else-if="meal.nom === 3">Dîner</p>
            <p v-if="meal.nom === 1">{{ getTotalCalories().breakfast }} kcal</p>
            <p v-else-if="meal.nom === 2">{{ getTotalCalories().lunch }} kcal</p>
            <p v-else-if="meal.nom === 3">{{ getTotalCalories().dinner }} kcal</p>
        </div>
        </div>

        <div class="meal-content">
            <button @click="passMealId(meal.id), showFoodMenu(), createMeal(meal.nom)">+</button>
        </div>
    </div>
</template>
<script>
    import { onMounted, ref} from "vue";
    export default {
        name: "MealItem",
        emits: ['meal-clicked', 'food-menu'],
        props: {
            foodMenu: Boolean
        },
        setup(props, context){
            class Meal{
                constructor(name, mealTime, icon){
                    this.name = name;
                    this.mealTime = mealTime;
                    this.icon = icon;
                }
            };
            let isFoodMenuActive = ref(props.foodMenu);
            const data_meal = ref([]);
            const BearerToken = "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJpYXQiOjE3MDIyMDQ5NTYsImV4cCI6MTcwMjI5MTM1Niwicm9sZXMiOlsiUk9MRV9VU0VSIl0sInVzZXJuYW1lIjoiYmFkdHJ1YzEwQGdtYWlsLmNvbSJ9.Plodo89A4Iadl2Q8_u3rjVyLuRh4yuw34eMbL_-6k-iZqKqzaKBXIlzGrg65luTO75Y4Ez_jsXnruV39YnhtkVJ5HUQuRVJmvVs-KHEKqUZ1YvB82ROHDUX0KOdfQciyLTxX88U0hbzjXj4ArNYnqs2cGiuwuXNLeKLa8JEG0DTMcDiuy4kfIJ_B0WJURE3h2y-QKMvrw0St-YqkvbFfPX1rBQDbQ-CAXEGl747iukBEZ0iJFf0CT8XDs_MN1CY-jyfw-29k70XJJ36TU6rvCAlyLcOnNWOoy2cKVvRdimZzkmirrbVl1aMfUEsq1q08c9Bowqbte5DLp4aqbYIi9Ml1Lms0N7bfX0hA6Ud6NnAoGbQs1kWraRef1WBTDFa5dZsiIQkhncNiojVQAUiN7j7980bubWwFmBV_by2nrKA8h1nsv0fdaXQI8o0CGOmqxYZBSYE_gkAF1X7c1fDfn7LEUtJDkT5MAHKI2RYT6jxgnSuGR6gg67M4_mEtr9Etz458rY5LJO_zHNB0J7QyaSvSfdxG98dXgfwQ8TY1e4RhBLDCNsqRjcLUX85ivQa-ikhoVEPXTFnc6j6GBkaub8JJkWywosLIW2x0OigTZ51xBhZSKzP0Tym-AfiuVxtlnXoyl0Lwx7iRhhHtkPzO1dcWwIx8_Q-nCUN9xvXgoHA";
            const makeMeal = async () => {
    try {
        // Récupération des meals avec une requête GET
        const getMealsUrl = 'http://127.0.0.1:8000/api/meals';
        const getMealsResponse = await fetch(getMealsUrl, {
            method: "GET",
            headers: {
                "Authorization": `Bearer ${BearerToken}`
            }
        });

        if (!getMealsResponse.ok) {
            throw new Error(`HTTP ERROR: ${getMealsResponse.status}`);
        }

        const getMealsData = await getMealsResponse.json();
        data_meal.value = getMealsData.sort((a, b) => a.nom - b.nom);
        console.log(data_meal.value);

        // Création de meals avec des noms différents en utilisant la méthode POST
        const postMealsUrl = 'http://127.0.0.1:8000/api/meals';
        const mealNames = [1, 2, 3];

        for (const name of mealNames) {
            // Vérifier si le meal avec le nom existe déjà
            const existingMeal = getMealsData.find(meal => meal.nom === name);

            if (existingMeal) {
                console.log(`Meal with name '${name}' already exists. Skipping creation.`);
            } else {
                // Créer le meal uniquement s'il n'existe pas encore
                const postMealResponse = await fetch(postMealsUrl, {
                    method: "POST",
                    headers: {
                        "Authorization": `Bearer ${BearerToken}`,
                        "Content-Type": "application/json"
                    },
                    body: JSON.stringify({ nom: name })
                });

                if (!postMealResponse.ok) {
                    throw new Error(`HTTP ERROR: ${postMealResponse.status}`);
                }

                const createdMealData = await postMealResponse.json();
                console.log('Meal created:', createdMealData);
            }
        }

    } catch (error) {
        console.error("Error fetching or creating meals:", error);
    }
}


            onMounted(() => {
                makeMeal();
            });

            const showFoodMenu = () => {
                if(isFoodMenuActive.value){
                    context.emit("food-menu", false);
                }else {
                    context.emit("food-menu", true);
                }
                
            };

            const createMeal = async (mealTemp) => {
                try {
                    const url = 'http://127.0.0.1:8000/api/meals';
                    const response = await fetch(url, {
                        method: "POST",
                        headers: {
                            "Authorization": `Bearer ${BearerToken}`
                        },
                        body: JSON.stringify({nom: mealTemp })
                    });

                    if(!response.ok){
                        throw new Error(`HTTP ERROR: ${response.status}`);
                    }

                    const data = await response.json();
                    console.log(data);
                }catch(error){
                    console.error("Error fetching data:", error);
                }
            }   


            return {
                data_meal,
                isFoodMenuActive,
                showFoodMenu,
                createMeal
            }
        },
        methods: {
    passMealId(mealId) {
        // Tu peux mettre ici le code que tu veux exécuter lorsque la méthode est appelée
        console.log("Meal ID:", mealId);
    },

    getTotalCalories() {
        let breakfastCalories = 0;
        let lunchCalories = 0;
        let dinnerCalories = 0;

        // Iterate over each meal in data_meal
        this.data_meal.forEach((meal) => {
            // Check if the meal has food items
            if (meal.food) {
                // Iterate over each food item in the meal
                meal.food.forEach((foodItem) => {
                    // Add the calories of each food item to the corresponding meal's total
                    switch (meal.nom) {
                        case 1:
                            breakfastCalories += foodItem.calory;
                            break;
                        case 2:
                            lunchCalories += foodItem.calory;
                            break;
                        case 3:
                            dinnerCalories += foodItem.calory;
                            break;
                        // Add more cases if needed for other meal types
                    }
                });
            }
        });

        // Return an object with the totals for each meal type
        return {
            breakfast: breakfastCalories,
            lunch: lunchCalories,
            dinner: dinnerCalories,
        };
    },

    getMealIcon(nom) {
        switch (nom) {
            case 1:
                return '../../public/meals-icons/chocolate.png';
            case 2:
                return '../../public/meals-icons/salad.png';
            case 3:
                return '../../public/meals-icons/soup.png';
            default:
                return '';  
        }
    },
}

    }

</script>