<script>
import Results from "./Results.vue"

export default {
    data(){
        return{
            meals: ""
        }
    },
    components: {
        Results: Results
    },
    methods: {
        get(j){
            if(j.meals != null){
                for (let index = 0; index < j.meals.length; index++) {

                    fetch(`https://www.themealdb.com/api/json/v1/1/lookup.php?i=${j.meals[index].idMeal}`)
                    .then(response => response.json())
                    .then(json => {
                        const datas = json

                        var list = []

                        for (let a = 1; a <= 20; a++) {

                            if(datas.meals[0]['strIngredient'+a] != "" && datas.meals[0]['strIngredient'+a] != null){
                                list.push({"ingredient": datas.meals[0]['strIngredient'+a], "mesure": datas.meals[0]['strMeasure'+a], "img": `https://www.themealdb.com/images/ingredients/${datas.meals[0]['strIngredient'+a]}.png`})
                            }
                        }

                        j.meals[index].listIngredient = list
                    })
                    .catch((e) => {
                        console.log(e);
                         
                    });
                }
            }
        },
        getRandom(){
            fetch(`https://www.themealdb.com/api/json/v1/1/random.php`)
            .then(response => response.json())
            .then(json => {
                this.meals = json
                console.log(json);

                this.get(this.meals)
            })
            .catch((e) => {
                console.log(e);
                 
            });
        }
    }
}
</script>

<template>
    <button @click="getRandom()" class="btn">Aléatoire</button>
    <Results :meals="meals"/>
</template>