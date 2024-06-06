<template>
  <div class="cardGrid">
    <Card v-for="card in cards" :card="card"></Card>
  </div>
</template>

<script setup>
import Card from "./UI/Card.vue";

const props = defineProps({ cards: Array });
</script>

<style scoped>
.cardGrid {
  display: grid;
  grid-template-columns: 45vw 45vw;
  justify-content: space-evenly;
}
</style>
