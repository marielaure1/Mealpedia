<script>
import Results from "./Results.vue"

export default {
    data(){
        return{
            meals: "",
            byCategory: "",
            message: "",
            listCategory: ""
        }
    },
    components: {
        Results: Results
    },
    methods: {
        getByCategory(){
           
            fetch(`https://www.themealdb.com/api/json/v1/1/filter.php?c=${this.byCategory}`)
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

                            this.meals.meals[index].listIngredient = list
                            this.meals.meals[index].strCategory = datas.meals[0].strCategory
                            this.meals.meals[index].strArea = datas.meals[0].strArea
                        })
                        .catch((e) => {
                            console.log(e);
                        });
                    }

                    if(this.meals.meals.length > 1){
                        this.message = `Il y a ${this.meals.meals.length} résultats.`
                    } else if(this.meals.meals.length == 1){
                        this.message = `Il y a 1 résultat.`
                    } else{
                        this.message = ""
                    }
                }

                if(this.meals.meals == null){
                    this.message = `Aucun plat ne correspond à votre recherche "${this.byCategory}"`
                }

                if(this.byCategory == ""){
                    this.message = "Veuillez choisir une catégorie"
                    this.meals = ""
                }

            })
            .catch((e) => {
                console.log(e);
            });
        }
    }, 
    mounted(){
        fetch(`https://www.themealdb.com/api/json/v1/1/list.php?c=list`)
        .then(response => response.json())
        .then(json => {
            this.listCategory = json
        })
        .catch((e) => {
            console.log(e);
            this.message = "Une erreur c'est produite veuillez réessayer plus tard."
        });

    }
}
</script>

<template>
    <select  v-model="byCategory" @change="getByCategory">
        <option value="" disabled selected hidden>Sélectionnez une catégorie</option>
        <option v-for="category in listCategory.meals">{{ category.strCategory
        }}</option>
    </select>
    <Results :meals="meals" :message="message"/>
</template>