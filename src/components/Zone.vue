<script>
import Results from "./Results.vue"

export default {
    data(){
        return{
            meals: "",
            byZone: "",
            message: "",
            listZone: ""
        }
    },
    components: {
        Results: Results
    },
    methods: {

        getByZone(){
           
            fetch(`https://www.themealdb.com/api/json/v1/1/filter.php?a=${this.byZone}`)
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
                    this.message = `Aucun plat ne correspond à votre recherche "${this.byZone}"`
                }

                if(this.byZone == ""){
                    this.message = "Veuillez choisir une zone"
                    this.meals = ""
                }

            })
            .catch((e) => {
                console.log(e);
                 
            });
        }
    }, 
    mounted(){
        fetch(`https://www.themealdb.com/api/json/v1/1/list.php?a=list`)
        .then(response => response.json())
        .then(json => {
            this.listZone = json
        })
        .catch((e) => {
            console.log(e);
             
        });

    }
}
</script>

<template>
    <select  v-model="byZone" @change="getByZone">
        <option value="" disabled selected hidden>Sélectionnez une zone</option>
        <option v-for="zone in listZone.meals">{{ zone.strArea }}</option>
    </select>
    <Results :meals="meals" :message="message"/>
</template>