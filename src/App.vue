<script setup>
import Nav from './components/Nav.vue'
// import TheWelcome from './components/TheWelcome.vue'
</script>

<template>
  <header>
    <div class="logo">
      <iconify-icon icon="game-icons:chef-toque"></iconify-icon>
      <h1>Mealpedia</h1>
    </div>
      
        <Nav/>
  </header>
  <img class="divider" src="./assets/wavy-loops-top.svg" alt="">

  <main>
    <router-view></router-view>
  </main>


</template>

<style scoped>

</style>
