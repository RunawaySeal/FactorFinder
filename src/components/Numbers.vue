<template>
	<!-- Added a title and description to explain the app to the user -->
	<div class="header">
		<h1>Find the Factors</h1>
		<p>Enter a number limit to generate a shuffled list of numbers. Hover over a number to highlight its factors.</p>
	</div>
	<div class="content">
		<input class="limit" type="number" v-model="limit" />
		<div class="numbers">
			<div v-for="num in numbers" 
					 :class="[{ active: num.highlighted }, 'number']"
			      @mouseover="highlightFactors(num.number)" 
						@mouseout="resetNumbers">
			  {{ num.number }}
		  </div>
		</div>
	</div>
</template>

<script setup lang="ts">
  import { ref, reactive, watch } from 'vue';
  import { INumber } from '../models/number';
  
	//Change properties to create a reactive state so we can update the view of the user(DOM) in real time
  const limit = ref(100);
  const numbers = reactive([] as INumber[]);
  
  //Change name to be more descriptive of action
  function generateNumbers() {
  	const nums = Array.from({ length: limit.value }, (_, i) => ({
  		number: i + 1,
  		highlighted: false
  	} as INumber)).sort(() => Math.random() - 0.5);
  
  	numbers.splice(0, numbers.length, ...nums);
  }
  
  //Change name to be more descriptive of action
	//Changed the highlighted functionality to be a property. So we use vue to control the adding the class functionality
  function highlightFactors(number: number) {
  	numbers.forEach((num) => {
  		num.highlighted = number % num.number === 0;
  	});
  }
  
  //Change name to be more descriptive of action
	function resetNumbers() {
  	numbers.forEach((num) => {
  		num.highlighted = false;
  	});
  }
  
	// watch is to regenerate numbers when the user changes the limit
  watch(limit, generateNumbers, { immediate: true });
</script>

<!-- Changed styling to make it a little more readable and user friendly -->
<!-- Added sass because I prefer using it -->
<style lang="scss">  
  .header {
  	display: flex;
  	flex-direction: column;
  }
  
  .content {
		display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
	}
  
  .number {  
  	display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
  	padding: 5px;
  	width: 50px;
  	height: 50px;
  	background-color: darkgray;
  	color: black;
  	margin: 5px;
  	cursor: context-menu;
  	border-radius: 50px;

		&.active {
  	  background-color: green;
    }
  }

  .numbers {
  	display: flex;
    flex-direction: row;
    flex-wrap: wrap;
  	font-size: 18px;
  }
    
  .limit {
  	color: #333;
  	padding: 10px;
  	font-size: 18px;
  	border: 2px solid #ccc;
  	border-radius: 10px;
  	background-color: #f9f9f9;
  	margin: 20px auto;
		&:focus {
  	  border-color: #007BFF;
  	  box-shadow: 0 0 10px rgba(0, 123, 255, 0.6);
  	  background-color: #fff;
    }
		&:hover {
  	  border-color: #0056b3;
    }
  } 
</style>
