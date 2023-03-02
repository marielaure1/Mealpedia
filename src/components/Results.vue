<script>
export default {
    props: ["meals", "message"],
    data(){
        return{
            name: "",
            category: "",
            area: "",
            thumb: "",
            ingredientsList: ""
        }
    },
    methods:{
        getIngredients(list){
            const ingredients = document.querySelector(`#ingredients`)
            if(ingredients){
                ingredients.classList.add("active")

                this.name = list.strMeal
                this.category = list.strCategory
                this.area = list.strArea
                this.thumb = list.strMealThumb
                this.ingredientsList = list.listIngredient
            }

            console.log(list);
        },

        closeIngredients(list){
            const ingredients = document.querySelector(`#ingredients`)
            if(ingredients){
                ingredients.classList.remove("active")

            }
        }
    }
}
</script>

<template>
    <div v-if="message" class="message">{{ message }}</div>
    <div class="results" v-if="meals.meals">

        <div class="result" v-for="meal of meals.meals" v-bind:style="{ 'background': 'url(' + meal.strMealThumb + ') center/cover' }" > 
            <h3>{{ meal.strCategory }}</h3> 
           
            <div class="content">
                <h2>{{ meal.strMeal }}</h2> 
    
                <button @click="getIngredients(meal)">Voir plus</button>
            </div>

            
           
        </div>
    </div>
    <div id="ingredients">
        <div class="container">
            <div class="head">
                <h3>{{ name }}</h3>
                <button @click="closeIngredients">
                    <iconify-icon icon="basil:cross-outline"></iconify-icon>
                </button>
            </div>
            <div class="corps">
                
                <div class="img">
                    <img :src="thumb" alt="">
                </div>
                
                <div class="listIngredients">
                    <p><span>{{ category }}</span> <span><iconify-icon icon="material-symbols:share-location-rounded"></iconify-icon> <span>{{ area }}</span></span></p>
                     <div class="content">
                        <div v-for="ing in ingredientsList">
                            <img :src="ing.img" alt="">
                            <h4>{{ ing.mesure }} {{ ing.ingredient }}</h4>
                            
                        </div>
                     </div>
                </div>
            </div>
        </div>
    </div>

</template>