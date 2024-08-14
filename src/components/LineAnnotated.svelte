<script>
    import * as d3 from "d3";
    
    export let stats;
    export let xAccessorScaled;
    export let yAccessorScaled;

    const interpolation = d3.curveMonotoneX;

    $: lineGenerator = d3.line()
        .defined(d => d.pitch !== 'NA' && d.pitch !== null && d.pitch !== undefined)
        .x(xAccessorScaled)
        .y(yAccessorScaled)
        .curve(interpolation);

    function getSegments(data) {
        let segments = [];
        let segment = [];
        let inRisePeakRange = false;

        for (let i = 0; i < data.length; i++) {
            if (data[i].rise_point === 1) {
                inRisePeakRange = true;
                segment = [data[i]]; // Start a new segment
            } else if (data[i].peak_point === 1 && inRisePeakRange) {
                segment.push(data[i]);
                segments.push(segment); // End the current segment
                inRisePeakRange = false;
                segment = [];
            } else if (inRisePeakRange) {
                segment.push(data[i]); // Continue the current segment
            }
        }

        return segments;
    }

    $: segments = getSegments(stats);

    function getColor(d) {
        return '#FF6961';
    }
</script>

<path 
    class="line" 
    d={lineGenerator(stats.filter(d => !d.rise_point && !d.peak_point))} 
    stroke="#545454"
/>

{#each segments as segment}
    <path 
        class="line" 
        d={lineGenerator(segment)} 
        stroke={getColor(segment[0])}
    />
{/each}


<style>
  .line {
    fill: none;
    stroke-width: 1.5px;
    stroke-linecap: round;
  }
</style>
