<template>
  <div id="app">
    <div id="boxContainer">
      <Box v-for="i in 9" :value="i" :key="i" :externalState="boxState[i-1]" @select="selectBox" />
    </div>


    <div id="diceContainer" @click="rollDice">
      <Die v-for="i in 2" :values="diceValues" :position="i"  :key="i"  />
    </div>
    <h1> Total: {{dieTotal}}</h1>
    <h1> Selected Total: {{selectedTotal}}</h1>
    <h1> Remainder: {{remainder}}</h1>

    <h2 @click="confirmBoxes"> Confirm? </h2>

    {{boxState}}
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
      diceValues: [0, 0],
      boxState: ["openSelectable", "openSelectable", "openSelectable", "openSelectable", "openSelectable", "openSelectable", "openSelectable", "openSelectable", "openSelectable"],
      possibleStates: ["openSelectable", "openInvalid", "selected", "used"]
    }
  },
  computed: {
    dieTotal(){
      return this.diceValues.reduce((a,b)=> {return a+b})
    },
    remainder(){
      return this.dieTotal - this.selectedTotal
    },
    selectedTotal(){
      let remaining = [0]
      this.boxState.forEach((item, index)=>{
        if (item === "selected") {
          remaining.push(index + 1)
        }
      })
      return remaining.length > 1 ? remaining.reduce((a,b)=> {return a+b}) : remaining[0]
    }
  },
  methods:{
    rollDice(value){
      let rollFirst = Math.floor(Math.random() * 6) + 1
      let rollSecond = Math.floor(Math.random() * 6) + 1
      Vue.set(this.diceValues, 0, rollFirst)
      Vue.set(this.diceValues, 1, rollSecond)
    },
    confirmBoxes(){
      this.boxState = this.boxState.map((item, index)=>{
        if (item === "selected" || item === "used") {
          return "used"
        } else {
          return "openSelectable"
        } 
      })
    },
    selectBox(value) {
      let currentState = this.boxState[value-1]
      if (currentState === "openSelectable") {
        Vue.set(this.boxState, value-1, 'selected')
      } else if (currentState === "selected") {
        Vue.set(this.boxState, value-1, 'openSelectable')
      } else {
        alert('not a valid option, dude')
      }
    }
  }
}
</script>

<style>
#boxContainer {
    margin-bottom: 1%;

}
</style>
