<script>
import { createRouter, createWebHistory } from 'vue-router';
import Home from "../components/Home.vue";
import Name from "../components/Name.vue";
import Random from "../components/Random.vue";
import Ingredient from "../components/Ingredient.vue";
import Category from "../components/Category.vue";
import Zone from "../components/Zone.vue";

export const routes = [
  { path: "/", component: Home , name: "home"},
  { path: "/random", component: Random, name: "random" },
  { path: "/name", component: Name, name: "name" },
  { path: "/ingredient", component: Ingredient, name: "ingredient" },
  { path: "/category", component: Category, name: "category" },
  { path: "/zone", component: Zone, name: "zone" },
]

const router = createRouter({
    
    history: createWebHistory(),
    routes, 
})

export default router
</script>