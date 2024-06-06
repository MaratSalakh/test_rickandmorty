<template>
  <div class="cardGrid">
    <Card :card="card" v-for="card in cards"></Card>
  </div>
</template>

<script>
import Card from "./UI/Card.vue";
export default {
  components: { Card },
  props: {
    cards: {
      type: Array,
      default: () => [],
    },
  },
};
</script>

<style scoped>
.cardGrid {
  display: grid;
  grid-template-columns: 45vw 45vw;
  justify-content: space-evenly;
}
</style>
