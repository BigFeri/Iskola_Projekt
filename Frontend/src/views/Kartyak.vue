<template>
  <div>
    <!-- Fej -->
    <h1>Kártyák</h1>
    <!-- Kártyák -->
    <div>
      <Cards :cards="cards" />
    </div>
    <!-- Paginator -->
    <div></div>
  </div>
</template>

<script>
import axios from "axios";
import Cards from '@/components/Cards.vue';
import Paginator from '@/components/Paginator.vue';

export default {
components:{
  Cards,
  Paginator
},
data() {
    return {
      cards: [],
      url: "http://localhost:8000/api",
      currentPage: 2, //melyik oldalon vagyunk
      cardsInPage: 3, //hány kártya lehet egy oldalon
      pagesArray: [], //az oldalak számok tömbje
      pagesNumber: 1, // összesen hány oldalunk van
    };
  },
  mounted() {
    this.getCards();
    this.getPagesNumber();
  },
  methods: {
    async getCards() {
      const url = `${this.url}/queryOsztalynevsorLimit/${this.currentPage}/${this.cardsInPage}`;
      const response = await axios.get(url);
      this.cards = response.data.data;
      console.log(this.cards);
    },
 
    async getPagesNumber() {
      const url = `${this.url}/queryHanyOldalVan/${this.cardsInPage}`;
      const response = await axios.get(url);
      this.pagesNumber = response.data.data.oldalszam;
      console.log(this.pagesNumber);
      this.pagesArray = [];
      for (let i = 0; i < this.pagesNumber; i++) {
        this.pagesArray.push(i + 1)
       
      }
    },
  }
}
 
</script>

<style>

</style>