<template>
 <div class="card" @click="selectCard">
  <div class="card-face is-front" v-if="visible">
    <img :src="`/images/${value}.png`" :alt="value" class="image"/>
  <img v-if="matched" src="../../public/images/check-mark.svg" class="icon-checkmark"/>

  </div>
  <div class="card-face is-back" v-else></div>
  
 </div>
</template>

<script>
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
    const selectCard = ()=>{
      context.emit('select-card', {
        position:props.position,
        faceValue:props.value
      })
    }
    return{
      selectCard
    }
  }


}
</script>

<style scoped>
.card{
  position: relative;
}
.card-face.is-front{
  background-color: rgb(123, 9, 135);
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
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
</style>
