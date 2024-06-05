<template>
  <div class="app">
    <CardsGrid>
      <Card :card="card" v-for="card in cards"></Card>
    </CardsGrid>
  </div>
</template>

<script>
import axios from "axios";
import CardsGrid from "./components/CardsGrid.vue";
import Card from "./components/UI/Card.vue";

export default {
  components: { Card, CardsGrid },
  data() {
    return { cards: [] };
  },
  methods: {
    async fetchCards() {
      try {
        const response = await axios.get(
          "https://rickandmortyapi.com/api/character"
        );
        console.log(response);
        this.cards = response.data.results;
      } catch (e) {
        alert("Error");
      }
    },
  },
  mounted() {
    this.fetchCards();
  },
};
</script>

<style scoped>
.app {
  background-color: rgb(39, 43, 51);
}
</style>
