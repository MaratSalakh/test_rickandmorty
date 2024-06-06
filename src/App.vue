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
          v-for="page in pages"
          :key="page"
          :class="{ currentPage: page === currentPage }"
          class="pageItem"
          @click="changePage(page)"
        >
          {{ page }}
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import axios from "axios";
import CardsGrid from "./components/CardsGrid.vue";
import MySelect from "./components/UI/MySelect.vue";
import { computed, onMounted, ref, watch } from "vue";

const cards = ref([]);
const selectedSort = ref("");
const sortOptions = [{ value: "name" }, { value: "status" }];
const currentPage = ref(1);
const pages = ref(1);
const sorting = ref(false);

async function fetchCards() {
  try {
    const response = await axios.get(
      "https://rickandmortyapi.com/api/character",
      {
        params: {
          page: currentPage.value,
        },
      }
    );
    console.log(response.data);
    cards.value = response.data.results;
    pages.value = response.data.info.pages;
  } catch (e) {
    alert("My error");
  }
}

function changePage(page) {
  currentPage.value = page;
}

function sortingOn() {
  sorting.value === true ? (selectedSort.value = "") : null;
  sorting.value = !sorting.value;
}

onMounted(() => {
  fetchCards();
});

const sortedCards = computed(() =>
  [...cards.value].sort((card1, card2) => {
    return card1[selectedSort.value]?.localeCompare(card2[selectedSort.value]);
  })
);

watch(currentPage, () => {
  fetchCards();
});
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
