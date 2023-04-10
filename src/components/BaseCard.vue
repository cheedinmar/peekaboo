<template>
 <div class="card" @click="selectCard" :class="flippedStyles">
  <div class="card-face is-front" >
    <img :src="`/images/${value}.png`" :alt="value" class="image"/>
  <img v-if="matched" src="/images/check-mark.svg" class="icon-checkmark"/>

  </div>
  <div class="card-face is-back"></div>
  
 </div>
</template>

<script>
import {computed} from 'vue'
export default {
  props:{
    value:{
      type:String,
      required:true
    },
    visible:{
      type: Boolean,
      default:false
    },
     matched:{
      type: Boolean,
      default:false
    },
    position:{
      type:Number,
      required:true
    }
  },
 
  setup(props, context){
    const flippedStyles = computed(()=>{
     return props.visible ? 'is-flipped': ''
   
    })
    const selectCard = ()=>{
      context.emit('select-card', {
        position:props.position,
        faceValue:props.value
      })
    }
    return{
      selectCard,
      flippedStyles
    }
  }


}
</script>

<style scoped>
.card{
  position: relative;
  transition: 0.5s transform ease-in;
  transform-style: preserve-3d;
  
}
.card.is-flipped{
  transform: rotateY(180deg);

}
.card-face.is-front{
  background-color: rgb(123, 9, 135);
  color: white;
  display: flex;
  width:100%;
  align-items: center;
  justify-content: center;
   transform: rotateY(180deg)
}
.card-face.is-back{
  background-image:url('../../public/images/card-bg-empty.png') ;
  background-color: yellow;
    background-size: cover;
  color: white;
 
}
.card-face{
  width:100%;
  height: 100%;
  position:absolute;
  border-radius: 10px;
  backface-visibility: hidden;
}
.icon-checkmark{
  position: absolute;
  right: 2px;
  bottom: 2px;
}
.image{
  width:55px;
  height:55px;
}
@media only screen and (max-width: 600px) {
  .image{
  width:35px;
  height:35px;
}
.card{
  width:20vw
}
}
</style>
