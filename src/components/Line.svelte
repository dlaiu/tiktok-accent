<script>
	import * as d3 from "d3";
	
	export let stats;
	export let xAccessorScaled;
	export let yAccessorScaled;

	const interpolation = d3.curveMonotoneX;

	// Function to filter out NA values for y
	// const filterNA = data => data.filter(d => d.pitch !== 'NA' && d.pitch !== null && d.pitch !== undefined);

	// Apply the filter to the data
	// $: filteredStats = filterNA(stats);

	$: lineGenerator = d3.line()
		.defined(d => d.pitch !== 'NA' && d.pitch !== null && d.pitch !== undefined)
		.x(xAccessorScaled)
		.y(yAccessorScaled)
		.curve(interpolation);

	$: line = lineGenerator(stats);

	console.log(yAccessorScaled)
</script>

<path 
	class="line" 
	d={line} 
/>

<style>
  .line {
    fill: none;
    stroke: #353535;
    stroke-width: 1.5px;
    stroke-linecap: round;
  }
</style>
