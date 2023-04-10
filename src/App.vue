<template>
  <img src="/images/26062e60b8-Peekaboo_Title.png" class="title" />
 <p class="description">A card matching game powered by Vue.js 3!</p>
  <button v-if="newPlayer" @click="startGame" class="button " style="margin-bottom: 20px;">
    <img src="/images/start.svg" alt="" />Start Game
  </button>
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

  <h2 class="status" style="padding:15px 0">
    {{ status }}
  </h2>
  
  <button v-if="!newPlayer" @click="restartGame" class="button" style="background-color: orange; margin-bottom;:15px ">
    <img src="/images/retart.svg" alt="restart" />Restart Game
  </button>
</template>

<script>
import BaseCard from "./components/BaseCard.vue";
import { ref, watch, computed } from "vue";
import _ from "lodash";
import {launchConfetti}from "@/utilities/confetti"
export default {
  components: {
    BaseCard,
  },
  setup() {
    const CardList = ref([]);
    const userSelection = ref([]);
    const newPlayer = ref(true)
    const startGame=()=>{
      newPlayer.value = false
      restartGame()
    }
    const status = computed(() => {
      if (remainingPairs.value == 0) {
        return "You Win";
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
        visible: false,
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
    watch(remainingPairs, currentVal =>{
      if(currentVal == 0){
        launchConfetti()
      }
    })
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
            }, 1000);
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
      startGame,
      newPlayer
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
font-family: 'Black Ops One', sans-serif;
                                                
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

  background-color: #00070c;
  min-height: 100vh;
  min-width:screen;
  height: 100%;
  color: #fff;
 
}
h2{
  margin:0
}
.game-board {
  display: grid;
  grid-template-columns: repeat(4, 120px);
  grid-column-gap: 40px;
  grid-template-rows: repeat(4, 120px);
  grid-row-gap: 30px;
  justify-content: center;
}
.title {
  width: 30vw;
  height: auto;
   padding-top: 35px;

}
.button{
  background-color: #41b883;
  color: white;
  padding: 0.75rem 0.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto;
  font-weight: bold;
  border-radius: 10px;
  border:0;
  font-size: 1.1rem;
}
.button img{
  padding-right: 5px;
}
.shuffle-card-move{
  transition : transform 0.8s ease-in;
  margin:0
}
@media only screen and (max-width: 600px) {
  .game-board {
  display: grid;
  grid-template-columns: repeat(4, 80px);
  grid-column-gap: 22px;
  grid-template-rows: repeat(4, 80px);
  grid-row-gap: 22px;
  justify-content: center;
}
.title{
  width: 50%;
  height: auto;
   padding-top: 45px;
}
}
</style>
