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
      :matched="card.matched"
    />
  </section>
  <h2>
    {{ status }}
  </h2>
  <button @click="shuffleCards"> Shuffle Cards</button>
</template>

<script>
import BaseCard from "./components/BaseCard.vue";
import { ref, watch, computed } from "vue";
import _ from "lodash"
export default {
  components: {
    BaseCard,
  },
  setup() {
    const CardList = ref([]);
    const userSelection = ref([]);
    const status =  computed(()=>{
      if (remainingPairs.value == 0){
        return 'players win'
      }else {
        return   `Remaining Pairs: ${remainingPairs.value}`
      }
    });
    const remainingPairs = computed(()=>{
      const remainingCards = CardList.value.filter(card=>card.matched == false).length
      return remainingCards/2
    })
    const shuffleCards = ()=>{
      CardList.value = _.shuffle(CardList.value)
    }
    for (let i = 0; i < 16; i++) {
      CardList.value.push({
        value: i,
        visible: true,
        position: i,
        matched: false,
      });
    }
    const flipCard = (payload) => {
      CardList.value[payload.position].visible = true;
      if (userSelection.value[0]) {
        userSelection.value[1] = payload;
      } else {
        userSelection.value[0] = payload;
      }
    };
    watch(
      userSelection,
      (currentVal) => {
        if (currentVal.length == 2) {
          const CardOne = currentVal[0];
          const CardTwo = currentVal[1];
          if (CardOne.faceValue == CardTwo.faceValue) {
            status.value = "Matched";
             CardList.value[CardOne.position].matched = true;
            CardList.value[CardTwo.position].matched = true;
          } else {
            status.value = "mismatch";
            CardList.value[CardOne.position].visible = false;
            CardList.value[CardTwo.position].visible = false;
          }

          userSelection.value.length = 0;
        }
      },
      { deep: true }
    );
    return {
      CardList,
      flipCard,
      userSelection,
      status,
      shuffleCards
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
