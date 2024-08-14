<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';

    export let scale; // xScale
    export let height; // height of the chart

    let xAxisElement;

    $: XAxis = d3.axisBottom(scale);
    $: d3.select(xAxisElement).call(XAxis);
 

    // onMount(() => {
    //     const xAxis = d3.axisBottom(scale);
    //     d3.select(xAxisElement).call(xAxis);
    // });
</script>

<g class="x-axis" bind:this={xAxisElement} transform={`translate(0, ${height})`} />

<style>
  .x-axis path,
  .x-axis line {
    fill: none;
    stroke: #000;
  }
  .x-axis text {
    font-size: 2em;
  }
</style>
