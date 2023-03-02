<script>
import Results from "./Results.vue"

export default {
    data(){
        return{
            meals: "",
            byIngredient: "",
            message: "",
            listIngredient: ""
        }
    },
    components: {
        Results: Results
    },
    methods: {

        getByIngredient(){
           
            fetch(`https://www.themealdb.com/api/json/v1/1/filter.php?i=${this.byIngredient}`)
            .then(response => response.json())
            .then(json => {
                this.meals = json

                if(this.meals.meals != null){
                    for (let index = 0; index < this.meals.meals.length; index++) {

                        fetch(`https://www.themealdb.com/api/json/v1/1/lookup.php?i=${this.meals.meals[index].idMeal}`)
                        .then(response => response.json())
                        .then(json => {
                            const datas = json

                            var list = []

                            for (let a = 1; a <= 20; a++) {

                                if(datas.meals[0]['strIngredient'+a] != "" && datas.meals[0]['strIngredient'+a] != null){
                                    list.push({"ingredient": datas.meals[0]['strIngredient'+a], "mesure": datas.meals[0]['strMeasure'+a], "img": `https://www.themealdb.com/images/ingredients/${datas.meals[0]['strIngredient'+a]}.png`})
                                }
                            }

                            console.log(this.meals.meals[index]);

                            this.meals.meals[index].listIngredient = list
                            this.meals.meals[index].strCategory = datas.meals[0].strCategory
                            this.meals.meals[index].strArea = datas.meals[0].strArea
                        })
                        .catch((e) => {
                            console.log(e);
                             
                        });
                    }
                }

                this.message = ""

                if(this.meals.meals == null){
                    this.message = `Aucun plat ne correspond à votre recherche "${this.byIngredient}"`
                }

                if(this.byIngredient == ""){
                    this.message = "Veuillez choisir un ingrédient"
                    this.meals = ""
                }

            })
            .catch((e) => {
                console.log(e);
                 
            });
        }
    }, 
    mounted(){
        fetch(`https://www.themealdb.com/api/json/v1/1/list.php?i=list`)
        .then(response => response.json())
        .then(json => {
            this.listIngredient = json
            console.log(json);
        })
        .catch((e) => {
            console.log(e);
             
        });

    }
}
</script>

<template>
    <select  v-model="byIngredient" @change="getByIngredient">
        <option value="" disabled selected hidden>Sélectionnez un ingrédient</option>
        <option v-for="ingredient in listIngredient.meals">{{ ingredient.strIngredient
        }}</option>
    </select>
    <Results :meals="meals" :message="message"/>
</template>