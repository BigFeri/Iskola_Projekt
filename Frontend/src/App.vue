<script>
import { RouterLink, RouterView } from 'vue-router'
export default {

}

</script>

<template>
  <div class="container-fluid my-border my-container">

    
    <nav class="my-border p-4">
      <RouterLink to="/">Home</RouterLink> | 
      <RouterLink to="/kartyak">Cards</RouterLink> 

    </nav>
    
    <div class="my-border p-2">
      <RouterView />
    </div>
  </div>
</template>

<style scoped>

</style>


