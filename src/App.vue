<template>
  <img src="/images/26062e60b8-Peekaboo_Title.png" class="title" />
 
    <transition-group tag="section" class="game-board" name="shuffle-card">
      <base-card
      v-for="(card) in CardList"
      :value="card.value"
      :visible="card.visible"
      :key="`card-${card.value}-${card.variant}`"
      :position="card.position"
      @select-card="flipCard"
      :matched="card.matched"
    />
    </transition-group>

  <h2>
    {{ status }}
  </h2>
  <button @click="restartGame" class="button">
    <img src="/images/retart.svg" alt="restart" />Restart Game
  </button>
</template>

<script>
import BaseCard from "./components/BaseCard.vue";
import { ref, watch, computed } from "vue";
import _ from "lodash";
export default {
  components: {
    BaseCard,
  },
  setup() {
    const CardList = ref([]);
    const userSelection = ref([]);
    const status = computed(() => {
      if (remainingPairs.value == 0) {
        return "players win";
      } else {
        return `Remaining Pairs: ${remainingPairs.value}`;
      }
    });
    const remainingPairs = computed(() => {
      const remainingCards = CardList.value.filter(
        (card) => card.matched == false
      ).length;
      return remainingCards / 2;
    });

    const cardItems = [
      "angular",
      "c++",
      "vue",
      "html",
      "laravel",
      "python",
      "ruby",
      "typescript",
    ];
    cardItems.forEach((item) => {
      CardList.value.push({
        value: item,
        variant:1,
        visible: true,
        position: null,
        matched: false,
      });
      CardList.value.push({
        value: item,
        variant:2,
        visible: true,
        position: null,
        matched: false,
      });
    });
    CardList.value = CardList.value.map((card, index) => {
      return {
        ...card,
        position: index,
      };
    });

    const flipCard = (payload) => {
      CardList.value[payload.position].visible = true;
      if (userSelection.value[0]) {
        if (
          userSelection.value[0].position === payload.position &&
          userSelection.value[0].faceValue === payload.faceValue
        ) {
          return;
        } else {
          userSelection.value[1] = payload;
        }
      } else {
        userSelection.value[0] = payload;
      }
    };
    const restartGame = () => {
     CardList.value = _.shuffle(CardList.value);
      CardList.value = CardList.value.map((card, index) => {
        return {
          ...card,
          matched: false,
          position: index,
          visible: false,
        };
      });
    };
    watch(
      userSelection,
      (currentVal) => {
        if (currentVal.length == 2) {
          const CardOne = currentVal[0];
          const CardTwo = currentVal[1];
          if (CardOne.faceValue == CardTwo.faceValue) {
            CardList.value[CardOne.position].matched = true;
            CardList.value[CardTwo.position].matched = true;
          } else {
            setTimeout(() => {
              CardList.value[CardOne.position].visible = false;
              CardList.value[CardTwo.position].visible = false;
            }, 2000);
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
      restartGame,
    };
  },
};
</script>

<style>
html,
body {
  margin: 0;
  padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

  background-color: #00070c;
  min-height: 100vh;
  color: #fff;
  padding-top: 40px;
}

.game-board {
  display: grid;
  grid-template-columns: repeat(4, 100px);
  grid-column-gap: 22px;
  grid-template-rows: repeat(4, 100px);
  grid-row-gap: 22px;
  justify-content: center;
}
.title {
  width: 50%;
  height: auto;
}
.button{
  background-color: orange;
  color: white;
  padding: 0.75rem 0.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto;
  font-weight: bold;
  border-radius: 10px;
}
.button img{
  padding-right: 10px;
}
.shuffle-card-move{
  transition : transform 0.8s ease-in 
}
</style>
