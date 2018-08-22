<template>
  <div id="app">
  <transition name="over">
    <div id="gameOver" v-show="gameOver">
      Game Over!
    </div>
  </transition>

    <div id="boxContainer">
      <Box v-for="i in 9" :value="i" :key="i" :externalState="boxState[i-1]" @select="selectBox" :class="boxState[i-1]" />
    </div>

    <br />
      <Die v-for="i in 2" :values="diceValues" :position="i"  :key="i" />

    <h3> Dice Total: {{diceTotal}}</h3>
    <h3> Selected Total: {{selectedTotal}}</h3>
    <h3> Remainder From Selection: {{remainder}}</h3>
      
    <p><button class="button" :class="{disabled: !canIRoll}" :disabled="!canIRoll" @click="rollDice">Roll the dice!</button></p>
    <p><button class="button" :class="{disabled: !canIConfirm}" :disabled="!canIConfirm" @click="confirmBoxes"> Confirm Selections? </button>
    </p>

<br />

    <p><button class="button" @click="refresh"> Restart game </button></p>
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
      canIRoll: true,
      canIConfirm: false,
      gameOver: false
    }
  },
  computed: {
    diceTotal(){
      return this.diceValues.reduce((a,b)=> {return a+b})
    },
    remainder(){
      return this.diceTotal - this.selectedTotal
    },
    selectedTotal(){
      let remaining = [0]
      this.boxState.forEach((item, index)=>{
        if (item === "selected") {
          remaining.push(index + 1)
        }
      })
      return remaining.length > 1 ? remaining.reduce((a,b)=> {return a+b}) : remaining[0]
    },
    availableArray(){
      var availableArray = []
      this.boxState.forEach((item, index)=>{
        if (item !== "used") {
          availableArray.push(index+1)
        }
      })
      return availableArray
    },
    totalLeftovers() {
      return this.availableArray.reduce((a,b)=>{return a+b})
    }
  },
  beforeMount(){
    this.rollDice()
  },
  methods:{
    refresh(){
      this.canIRoll = true
      this.canIConfirm = false
      this.gameOver = false
      this.diceValues = [0, 0]
      this.boxState = ["openSelectable", "openSelectable", "openSelectable", "openSelectable", "openSelectable", "openSelectable", "openSelectable", "openSelectable", "openSelectable"]
      this.rollDice()    
    },
    rollDice(){
      let rollFirst = Math.floor(Math.random() * 6) + 1
      let rollSecond = Math.floor(Math.random() * 6) + 1
      Vue.set(this.diceValues, 0, rollFirst)
      Vue.set(this.diceValues, 1, rollSecond)
      this.canIRoll = false
    },
    confirmBoxes(){
      this.canIConfirm = false
      this.boxState = this.boxState.map((item)=>{
        if (item === "selected" || item === "used") {
          return "used"
        } else {
          return "openSelectable"
        } 
      })
      this.canIRoll = true
    },
    selectBox(value) {
      let currentState = this.boxState[value-1]
      if (currentState === "openSelectable") {
        Vue.set(this.boxState, value-1, 'selected')
      } else if (currentState === "selected") {
        this.boxState = this.boxState.map((item, index)=>{
        if (item === "selected" && index !== value -1) {
          return "selected"
        } else if (item === "used") {
            return "used"
          } else {
            return "openSelectable"
          }
        })
      } else {
        alert('not a valid option, dude')
      }
    },
    combinations(array, n) {
      // https://stackoverflow.com/questions/21640437/return-all-possible-combinations-of-numbers-in-an-array-whose-sum-is-less-than-o#comment32706278_21640668
      var lists = [], i;
      var picker = function(p,c,k) {
          if( 1<<k & i ) p.push(c);
          return p;
      };
      for( i=1 ; i < 1<<array.length ; ++i )
      {
          var sublist = array.reduce(picker,[]);
          if( sublist.reduce(function(p,c) { return p+c },0) == n )
              lists.push(sublist);
      }
      return lists;
  }

  },
  watch: {
    remainder(value) {
      if (this.canIRoll == false) {

        if (value === 0) {
          this.boxState = this.boxState.map(item=>{
            if (item === "openSelectable") {
              return "openInvalid"
            } else {
              return item
            }
          })
          this.canIConfirm = true
        } else {

          
          var possibleCombos = this.combinations(this.availableArray, value)
          
          if (possibleCombos.length < 1 && !this.canIRoll) {

            this.gameOver = true
          } else {  
            var possibleFlattened = possibleCombos.reduce((prev, curr) => {
              return prev.concat(curr);
            })

            this.boxState = this.boxState.map((item, index)=>{
              if (item !== "selected" && item !== "used" && !possibleFlattened.includes(index + 1)) {
                return "openInvalid"
              } else {
                return item
              }
            })
          }

        }
      }


    },
    boxState(value) {
      var selectable = [0]
      value.forEach((item, index)=>{
        if (item === "openSelectable") {
          selectable.push(index+1)
        }
      })
    },
    totalLeftovers() {
      if (this.totalLeftovers < this.diceTotal) {
        this.gameOver = true
        // this.refresh
      }
    }
  }
}
</script>

<style>

#app {
    text-align: center;
}

#boxContainer {
    margin-bottom: .5%;
    width: 100%;
}

box-component {
    width: 10vw;
}

.button {
    background-color: grey; /* Green */
    border: none;
    color: white;
    border-radius: 8px;
    padding: 15px 32px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 1rem;
}

.button.disabled {
  opacity: 0.6;
}

#gameOver {
  font-size: 3rem; 
  background-color:#4B0082; 
  color:white;
  margin-bottom: 1%
}

.over-enter-active, .over-leave-active {
  transition: opacity 1s 
}
.over-enter, .over-leave-to {
  opacity: 0;
}

.imageTransition-enter-active {
    animation: shift-in 2s;
}
.imageTransition-leave-active {
  animation: shift-in 2s reverse;
}

@keyframes shift-in {
    0%   {transform:rotate(0deg);}
    25%  {transform:rotate(90deg);}
    50%  {transform:rotate(120deg);}
    75%  {transform:rotate(180deg);}
    100% {transform:rotate(360deg); }
}

.imageTransition-enter, .imageTransition-leave-to {
  opacity: 0;
}

</style>
