<template>

  <img :src="'http://dobbelsteen.virtuworld.net/img/' + myValue + 'c.gif'">
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
      let value = this.values[this.position - 1]
      return value > 0 ? value : Math.floor(Math.random() * 6) + 1
    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
img {
  max-height: 150px;

}
/*.die {
  border: 3px dotted black;
  padding: 4px;
  display:inline-block;
}*/
</style>
