<template>
    <div>
      <div class="row row-cols-1 row-cols-md-2 g-4">
        <div class="col" v-for="(card, index) in cards" :key="index">
          <div class="card">
            <div class="card-body">
              <h5 class="card-title"></h5>
              <p class="card-text">
              </p>
            </div>
          </div>
        </div>
        
      </div>
    </div>
  </template>
  
  <script>
  export default {};
  </script>
  
  <style>
  </style>