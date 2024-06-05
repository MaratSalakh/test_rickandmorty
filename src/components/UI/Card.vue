<template>
  <div class="card">
    <div><img class="img" :src="card.image" :alt="card.name" /></div>
    <div class="info">
      <div>
        <h2 class="cardName">{{ card.name }}</h2>
        <div class="status">
          <div v-if="card.status === 'Dead'" class="redDot"></div>
          <div v-else-if="card.status === 'Alive'" class="greenDot"></div>
          <div v-else class="greyDot"></div>
          <p>{{ card.status }} - {{ card.species }}</p>
        </div>
      </div>
      <div>
        <p class="secondText">Last known location:</p>
        <p class="content">{{ card.location.name }}</p>
      </div>
      <div>
        <p class="secondText">First seen in:</p>
        <p class="content">{{ card.origin.name }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    card: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style scoped>
.card {
  background-color: rgb(60, 62, 68);
  border-radius: 10px;
  margin: 10px;

  color: white;

  display: flex;
}
.secondText {
  color: rgb(158, 158, 158);
}
.img {
  max-width: 250px;
  max-height: 250px;
  border-top-left-radius: 10px;
  border-bottom-left-radius: 10px;
}
.info {
  display: flex;
  flex-flow: column;
  justify-content: space-around;
  margin-left: 15px;
}
.content {
  font-size: large;
}
.cardName {
  font-size: xx-large;
}
.redDot {
  height: 10px;
  width: 10px;
  border-radius: 100%;
  background-color: red;
  margin-right: 5px;
}
.greenDot {
  height: 10px;
  width: 10px;
  border-radius: 100%;
  background-color: green;
  margin-right: 5px;
}
.greyDot {
  height: 10px;
  width: 10px;
  border-radius: 100%;
  background-color: rgb(158, 158, 158);
  margin-right: 5px;
}
.status {
  display: flex;
  align-items: center;
}
</style>
