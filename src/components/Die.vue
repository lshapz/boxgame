<template>
  <transition name="imageTransition" mode="out-in" appear css> 
    <img :src="'http://dobbelsteen.virtuworld.net/img/' + myValue + 'c.gif'" :key="myValue">
  </transition>
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
<style>

img {
  height: 100px;
  max-height: 115px;
  padding-left: 20px;
}

.imageTransition-appear-active, .imageTransition-enter-active {
    animation: shift-in .15s;
}

@keyframes shift-in {
    0%   {transform:rotate(0deg);}
    25%  {transform:rotate(90deg);}
    50%  {transform:rotate(120deg);}
    75%  {transform:rotate(180deg);}
    100% {transform:rotate(360deg); }
}

</style>