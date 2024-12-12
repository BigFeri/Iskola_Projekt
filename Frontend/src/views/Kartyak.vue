<template>
  <div>
    <h1>Kártyák</h1>
  </div>
</template>

<script>
import axios from "axios";
import Cards from '@/components/Cards.vue';
import Paginator from '@/components/Paginator.vue';

export default {
components:{
  Cards
},
data(){
  return {
    cards: [],
    url: 'http://localhost:8000/api'
  }
},
mounted(){
  this.GetCards();
},
methods: {
  async GetCards(){
    const url = this.url;
    const response = await axios.get(url);
    this.cards = response.data.data;
    console.log(this.cards);
    
  }
}
}
</script>

<style>

</style>