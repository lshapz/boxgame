<template>
  <div id="app">
    <div id="boxContainer">
      <Box v-for="i in 9" :value="i" />
    </div>
    <div id="diceContainer" @click="rollDice">
      <Die v-for="i in 2" :values="diceValues" :position="i" />
    </div>
    <h1> {{dieTotal}}</h1>
  </div>
</template>

<script>
import Box from './components/Box.vue'
import Die from './components/Die.vue'
import Vue from 'vue'
export default {
  name: 'app',
  components: {
    Box,
    Die
  },
  data(){
    return {  
      diceValues: [0, 0]
    }
  },
  computed: {
    dieTotal(){
      return this.diceValues.reduce((a,b)=> {return a+b})
    },
    remainder(){
      return this.dieTotal - this.selectedTotal
    }
  },
  methods:{
    rollDice(value, position){
      let rollFirst = Math.floor(Math.random() * 6) + 1
      let rollSecond = Math.floor(Math.random() * 6) + 1
      Vue.set(this.diceValues, 0, rollFirst)
      Vue.set(this.diceValues, 1, rollSecond)

    }
  }
}
</script>

<style>
#boxContainer {
    margin-bottom: 1%;

}
</style>
