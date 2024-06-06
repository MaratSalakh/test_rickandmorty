<template>
  <div class="app">
    <h1 class="title">Rick and Morty</h1>
    <div class="buttons">
      <MySelect v-model="selectedSort" :options="sortOptions" />
      <button @click="sortingOn">
        {{ sorting === true ? "Отменить" : "Применить" }}
      </button>
    </div>
    <CardsGrid :cards="sorting === true ? sortedCards : cards"></CardsGrid>
    <div class="wrap_pagination">
      <div class="pagination">
        <div
          class="pageItem"
          :class="{ currentPage: page === currentPage }"
          @click="changePage(page)"
          :key="page"
          v-for="page in pages"
        >
          {{ page }}
        </div>
      </div>
    </div>
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
      sorting: false,

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
        this.allPagesGroups = Math.ceil(response.data.info.pages / this.pagesGroupStep);
      } catch (e) {
        alert("My error");
      }
    },
    changePage(page) {
      this.currentPage = page;
    },
    sortingOn() {
      this.sorting === true ? (this.selectedSort = "") : null;
      this.sorting = !this.sorting;
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
  flex-wrap: wrap;
  padding: 20px;
  width: 50vw;
}
.wrap_pagination {
  display: flex;
  justify-content: center;
}
.pageItem {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 50px;
  width: 50px;
  border: 1px solid rgb(158, 158, 158);
  cursor: pointer;
}
.currentPage {
  background-color: white;
}

.buttons {
  display: flex;
  justify-content: end;
  padding: 10px;
}

.redButton {
  background-color: red;
}
.greenButton {
  background-color: green;
}

.title {
  margin-left: 10px;
  color: white;
}
</style>
