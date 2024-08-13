<script>
    import { onMount } from "svelte";
    import * as d3 from "d3";
    // import data from '../data/7366018710248672517_pitch_values.json';
    export let data;
    export let fillColor;

    let svg_width;
    let svg_height = 80;

    let margins = {top: 20, bottom: 20, left:20, right:20}

    //chart dimensions 
	$: chartWidth = svg_width - margins.left - margins.right
	$: chartHeight = svg_height - margins.top - margins.bottom

  
    let midpoints = [];
  
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

    $: xScale = d3.scaleLinear().domain([0, d3.max(midpoints)]).range([0, chartWidth]);

  
    onMount(() => {
      findUptalkMidpoints(data);
    //   drawChart();
    });
  
    // function drawChart() {
    // //   const width = 800;
    // //   const height = 400;
  
    //   const svg = d3
    //     .select("#chart")
    //     .append("svg")
    //     .attr("width", width)
    //     .attr("height", height);
  
    //   const xScale = d3
    //     .scaleLinear()
    //     .domain([0, d3.max(midpoints)])
    //     .range([0, width]);
  
    //   svg
    //     .selectAll("circle")
    //     .data(midpoints)
    //     .enter()
    //     .append("circle")
    //     .attr("cx", d => xScale(d))
    //     .attr("cy", height / 2)
    //     .attr("r", 5)
    //     .attr("fill", {fillColor});
    // }
  </script>
  
  <!-- <div id="chart"></div> -->

  <div class="chart-space" bind:clientWidth = {svg_width}>
        <svg id="chart" width={svg_width} height={svg_height}>
            <g class = 'chart' transform = 'translate({margins.left}, {margins.top})'>
            {#each midpoints as midpoint}
                <circle cx={xScale(midpoint)} cy={chartHeight / 2} r="5" fill="#000000" />
            {/each}
            </g>
        </svg>
  </div>
  
  <style>
    .chart-space {
        background: lemonchiffon;
    }
  </style>