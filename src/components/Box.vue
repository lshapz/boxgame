<template>
  <span class="box" @click="selectMe">
   	{{value}} {{myState}}
  </span>
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
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.box {
	padding: 1px;
	border: 1px solid red;
  width: 20px;
  height: 20px;
}
</style>
