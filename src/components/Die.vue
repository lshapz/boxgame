<template>
  <span class="die">
   {{myValue}}
  </span>
</template>

<script>
export default {
  name: 'Die',
  props: {
    values: Array,
    position: Number
  },
  computed: {
    myValue(){
      return this.values[this.position - 1]
    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.die {
  border: 3px dotted black;
  padding: 4px;
}
</style>
