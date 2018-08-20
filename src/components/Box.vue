<template>
  <span class="box">
   	{{value}}
  </span>
</template>

<script>
export default {
  name: 'Box',
  props: {
    value: Number
  },
  data(){
  	return {
  		used: false,
	    openSelectable: true,
	    openInvalid: false,
	    selected: false
  	}
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.box {
	padding: 1px;
	border: 1px solid red;
}
</style>
