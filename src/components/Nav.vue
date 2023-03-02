<template>
    <nav>
        <router-link :to="{ name: 'home'}">Accueil</router-link>
        <router-link :to="{ name: 'random'}">Plat Aléatoire</router-link>
        <router-link :to="{ name: 'name'}">Rechercher par nom</router-link>
        <router-link :to="{ name: 'ingredient'}">Recherche par ingrédient</router-link>
        <router-link :to="{ name: 'category'}">Recherche par catégorie</router-link>
        <router-link :to="{ name: 'zone'}">Recherche par zone</router-link>
      
    </nav>
</template>
