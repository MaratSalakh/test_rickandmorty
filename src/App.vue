<template>
  <div class="app">
    <div class="select">
      <MySelect v-model="selectedSort" :options="sortOptions" />
    </div>
    <CardsGrid :cards="sortedCards"></CardsGrid>
    <div class="pagination">
      <div
        class="pageItem"
        :class="{ currentPage: page === currentPage }"
        @click="changePage(page)"
        :key="page"
        v-for="page in 20"
      >
        {{ page }}
      </div>
    </div>
    <!-- <Pagination :currentPage="currentPage" :pages="pages" /> -->
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
      pages: 1,
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
        this.pages = response.data.info.pages;
      } catch (e) {
        alert("My error");
      }
    },
    changePage(page) {
      this.currentPage = page;
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
  watch: {
    currentPage() {
      this.fetchCards();
    },
  },
};
</script>

<style lang="scss" scoped>
.app {
  background-color: rgb(39, 43, 51);
}

.pagination {
  color: rgb(158, 158, 158);
  display: flex;
  justify-content: center;
  flex-flow: row;
}
.pageItem {
  padding: 10px;
  border: 1px solid rgb(158, 158, 158);
}
.currentPage {
  background-color: white;
}

.select {
  display: flex;
  justify-content: end;
}
</style>
