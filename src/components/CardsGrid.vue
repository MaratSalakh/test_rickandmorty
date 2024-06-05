<template>
  <h1 class="title">The Rick and Morty</h1>
  <div class="cardGrid"><slot></slot></div>
</template>

<script>
export default {};
</script>

<style scoped>
.cardGrid {
  display: grid;
  grid-template-columns: 45vw 45vw;
  justify-content: space-evenly;
}
.title {
  margin-left: 10px;
  color: white;
}
</style>
