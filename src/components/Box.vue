<template>
  <div class="box" @click="selectMe">
    {{value}}
  </div>
</template>

<script>
// import Vue from 'vue'

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
<style>
.box {
  padding: 1px;
  width: 10vw;
  height: auto;
  display: inline-block;
  text-align: center;
  font-size: 3rem;
  transition: 1.5s;
}

.box.openSelectable {
  border: 1px solid green;
  background-color: green;
  color: white;
}

.box.openInvalid {
  border: 1px solid red;
  background-color: red;
  color: white;
}

.box.selected {
  border: 1px solid blue;
  background-color: blue;
  color: white;
}

.box.used {
  border: 1px solid black;
  background-color: black;
  color: black;
}
.box.used:hover {
  color: white;
}
</style>