<template>
 <div class="card" @click="selectCard">
  <div class="card-face is-front" v-if="visible">{{ value }}</div>
  <div class="card-face is-back" v-else>Back</div>
  
 </div>
</template>

<script>
export default {
  props:{
    value:{
      type:Number,
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
  border: 5px solid #ccc;
  position: relative;
}
.card-face.is-front{
  background-color: red;
  color: white;
}
.card-face.is-back{
  background-color: blue;
  color: white;
}
.card-face{
  width:100%;
  height: 100%;
  position:absolute;

}
</style>
