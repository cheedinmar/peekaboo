<template>
  <h1>Peek a vue</h1>
  <section class="game-board">
   <base-card v-for="(card, index) in CardList" :value="card" :key="`card-${index}`"/>
  </section>
</template>

<script>
import BaseCard from './components/BaseCard.vue';
export default {
  components:{
    BaseCard
  },
  setup(){
    const CardList =[];
    for (let i=0; i<16; i++){
      CardList.push(i)
    }
    return{
      CardList
    }
  }
}
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
.card{
  border: 5px solid #ccc
}
.game-board{
  display: grid;
  grid-template-columns: 100px 100px 100px 100px;
  grid-column-gap:30px ;
  grid-template-rows:100px 100px 100px 100px;
  grid-row-gap: 30px;
  justify-content: center;
}
</style>
