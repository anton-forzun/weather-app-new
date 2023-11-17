<script setup>
import { RouterLink, RouterView } from "vue-router";

</script>

<template>
  <div>
    <header>
      <img alt="Vue logo" class="logo" src="@/assets/cloud_1.png" />
      <h3>Weather App</h3>
      <nav>
        <!-- <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/favourites">Favorite</RouterLink> -->
      </nav>
    </header>
  </div>

  <RouterView />
</template>


<style scoped>
header {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
}
.logo {
  max-width: 200px;
  width: 100%;
}
</style>
