<template>
  <div class="app">
    <MySelect v-model="selectedSort" :options="sortOptions" />
    <CardsGrid :cards="sortedCards"></CardsGrid>
  </div>
</template>

<script>
import axios from "axios";
import CardsGrid from "./components/CardsGrid.vue";
import MySelect from "./components/UI/MySelect.vue";

export default {
  components: { CardsGrid, MySelect },
  data() {
    return {
      cards: [],
      selectedSort: "",
      sortOptions: [{ value: "name" }, { value: "status" }],
      currentPage: 1,
    };
  },
  methods: {
    async fetchCards() {
      try {
        const response = await axios.get(
          "https://rickandmortyapi.com/api/character",
          {
            params: {
              page: this.currentPage,
            },
          }
        );
        console.log(response.data);
        this.cards = response.data.results;
      } catch (e) {
        alert("Error");
      }
    },
  },
  mounted() {
    this.fetchCards();
  },
  computed: {
    sortedCards() {
      return [...this.cards].sort((card1, card2) =>
        card1[this.selectedSort]?.localeCompare(card2[this.selectedSort])
      );
    },
  },
};
</script>

<style scoped>
.app {
  background-color: rgb(39, 43, 51);
}
</style>
