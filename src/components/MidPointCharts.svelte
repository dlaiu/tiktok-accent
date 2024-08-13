<script>
	import { onMount } from 'svelte';
	import * as d3 from 'd3';
	import { tweened } from 'svelte/motion';
	import { cubicOut } from 'svelte/easing';

	export let data;
	export let fillColor;
	export let currentTime;

	let svg_width;
	let svg_height = 80;

	let margins = { top: 20, bottom: 20, left: 20, right: 20 };

	//chart dimensions
	$: chartWidth = svg_width - margins.left - margins.right;
	$: chartHeight = svg_height - margins.top - margins.bottom;

	let midpoints = [];
	let activeIndex = -1;

    let time = data.map((d) => d.time)
    let maxTime = d3.max(time);

	function findUptalkMidpoints(data) {
		let start = null;

		data.forEach((d, i) => {
			if (d.rise_point === 1) {
				start = d.time;
			} else if (start !== null && d.peak_point === 1) {
				let midpoint = (start + d.time) / 2;
				midpoints.push(midpoint);
				start = null; // Reset start for the next potential uptalk
			}
		});

		console.log(midpoints);
	}

	$: xScale = d3
		.scaleLinear()
		.domain([0, maxTime])
		.range([0, chartWidth]);

	// Update activeIndex based on currentTime
	$: {
		activeIndex = midpoints.findIndex((midpoint) => Math.abs(midpoint - currentTime) < 0.05);
	}

	// let cy = tweened(chartHeight / 2, { duration: 200, easing: cubicOut });
	// let radius = tweened(5, { duration: 200, easing: cubicOut });
	// let color = tweened(fillColor || '#000000', { duration: 200, easing: cubicOut });

	// $: {
	// 	if (activeIndex !== -1) {
	// 		cy.set(chartHeight / 2 - 20);
	// 		radius.set(10);
	// 		color.set('red');
	// 	} else {
	// 		cy.set(chartHeight / 2);
	// 		radius.set(5);
	// 		color.set(fillColor || '#000000');
	// 	}
	// }

	onMount(() => {
		findUptalkMidpoints(data);
	});
</script>

<div class="chart-space" bind:clientWidth={svg_width}>
	<svg id="chart" width={svg_width} height={svg_height}>
		<g class="chart" transform="translate({margins.left}, {margins.top})">
			{#each midpoints as midpoint, i}
				<circle 
                    cx={xScale(midpoint)} 
                    cy={Math.abs(midpoint - currentTime) < 0.05 ? chartHeight / 2 - 20 : chartHeight / 2}
                    r={Math.abs(midpoint - currentTime) < 0.05 ? 10 : 5}
                    fill={Math.abs(midpoint - currentTime) < 0.05 ? "red" : fillColor || "#000000"}
                    style="transition: all 0.2s ease-out;"
                />
			{/each}
		</g>
	</svg>
</div>

<style>
	.chart-space {
		background: lemonchiffon;
	}

    circle {
        transition: all 0.2s ease-out;
    }
</style>
