<template>
  <h1>Peek a vue</h1>
  <section class="game-board">
    <base-card
      v-for="(card, index) in CardList"
      :value="card.value"
      :visible="card.visible"
      :key="`card-${index}`"
      :position="card.position"
      @select-card="flipCard"
    />
  </section>
</template>

<script>
import BaseCard from "./components/BaseCard.vue";
import {ref} from "vue"
export default {
  components: {
    BaseCard,
  },
  setup() {
    const CardList = ref([]);
    for (let i = 0; i < 16; i++) {
      CardList.value.push({
        value: i,
        visible: false,
        position:i
      });
    }
    const flipCard = (payload)=>{
      CardList.value[payload.position].visible= true
    }
    return {
      CardList,flipCard
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.card {
  border: 5px solid #ccc;
}
.game-board {
  display: grid;
  grid-template-columns: 100px 100px 100px 100px;
  grid-column-gap: 30px;
  grid-template-rows: 100px 100px 100px 100px;
  grid-row-gap: 30px;
  justify-content: center;
}
</style>
