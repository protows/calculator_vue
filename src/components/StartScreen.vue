<template>
  <div class="alert alert-secondary">
  	<h3>Are you ready?</h3>
  	<button class="btn btn-success" @click="$emit('onStart')">
  	Start
  </button>
  </div> 
</template>

<script>
	export default{
		methods: {
			some(){
				alert(1);
			}
		}
	}
</script>

<style scoped>
 	.alert{
 		text-align: center;
 		background-color: #C0C0C0;
 	}
 	h3, .btn{
 		margin: 30px 0;
 	}
 	
</style>
