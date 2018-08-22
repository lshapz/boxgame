<template>
  <div class="box" :class="classObj" @click="selectMe">
   	{{value}}
  </div>
</template>

<script>
import Vue from 'vue'

export default {
  name: 'Box',
  props: {
    value: Number,
    externalState: String
  },
  data(){
  	return {
  		myState: "openSelectable"
  	}
  },
  watch: {
    externalState(newVal) {
      this.myState = newVal
    }
  },
  methods: {
    selectMe() {
      this.$emit('select', this.value)
    }
  },
  computed: {
    classObj(){
      return {
        usedBox: this.externalState == 'used',
        selectedBox: this.externalState == 'selected',
        invalidBox: this.externalState == 'openInvalid',
        selectableBox: this.externalState == 'openSelectable'
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.box {
  padding: 1px;
  width: 10vw;
  height: auto;
  display: inline-block;
  text-align: center;
  font-size: 3rem;
}

.box.selectableBox {
  border: 1px solid green;
  background-color: green;
  color: white;
}

.box.invalidBox {
  border: 1px solid red;
  background-color: red;
  color: white;
}

.box.selectedBox {
  border: 1px solid blue;
  background-color: blue;
  color: white;
}

.box.usedBox {
  border: 1px solid black;
  background-color: black;
  color: white;
}



</style>
