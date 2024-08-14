<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';
    import Line from './Line.svelte';
    import LineAnnotated from './LineAnnotated.svelte';

    export let data;
    export let currentTime;
    export let fillColor;

    let svg_width;
    let svg_height = 300;

    let margins = { top: 20, bottom: 20, left: 40, right: 20 };

    // chart dimensions
    $: chartWidth = svg_width - margins.left - margins.right;
    $: chartHeight = svg_height - margins.top - margins.bottom;

    let time = data.map((d) => d.time);
    let pitch = data.map((d) => d.pitch);

    let maxTime = d3.max(time);
    let maxPitch = d3.max(pitch);
    let minPitch = d3.min(pitch);

    $: xScale = d3.scaleLinear().domain([0, maxTime]).range([0, chartWidth]);
    $: yScale = d3.scaleLinear().domain([minPitch, maxPitch]).range([chartHeight, 0]);

    // Accessors
    const xAccessor = d => d.time;
    const yAccessor = d => d.pitch;

    $: xAccessorScaled = d => xScale(xAccessor(d));
    $: yAccessorScaled = d => yScale(yAccessor(d));

    // Calculate playhead position based on current time
    $: playheadPosition = xScale(currentTime);

    // Midpoint calculation
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
    }

    onMount(() => {
        findUptalkMidpoints(data);
    });

    // Tolerance for midpoint bounce trigger
    let tolerance = 0.1; // 100ms tolerance

    // Function to check if currentTime is close enough to a midpoint
    function isCloseToMidpoint(midpoint, currentTime) {
        return Math.abs(midpoint - currentTime) <= tolerance;
    }
</script>

<div class="chart-space" bind:clientWidth={svg_width}>
    <svg width={svg_width} height={svg_height}>
        <g class="chart" transform="translate({margins.left}, {margins.top})">
            <LineAnnotated stats={data} {xAccessorScaled} {yAccessorScaled} />

            <!-- Draw the midpoint circles -->
            {#each midpoints as midpoint}
                <circle 
                    cx={xScale(midpoint)} 
                    cy={isCloseToMidpoint(midpoint, currentTime) ? chartHeight / 2 - 20 : chartHeight / 2}
                    r={isCloseToMidpoint(midpoint, currentTime) ? 10 : 5}
                    fill={isCloseToMidpoint(midpoint, currentTime) ? "red" : fillColor || "#000000"}
                    style="transition: all 0.2s ease-out;"
                />
            {/each}

            <!-- Playhead line -->
            <line x1={playheadPosition} x2={playheadPosition} y1={0} y2={chartHeight} stroke="#FF6961" stroke-width="2" />
        </g>
    </svg>
</div>

<style>
    .chart-space {
        /* background: lemonchiffon; */
    }

    circle {
        transition: all 0.2s ease-out;
    }
</style>
