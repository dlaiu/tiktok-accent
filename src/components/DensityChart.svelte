<script>
	import { onMount } from 'svelte';
	import * as d3 from 'd3';
	// import data from './data.js'
	import DensityLine from './DensityLine.svelte';
	import XAxis from './XAxis.svelte';

	import Amazon from '../data/amazonfinds_intonations.json';
	import Bookrecs from '../data/bookrecs_intonations.json';
	import Grwm from '../data/grwm_intonations.json';
	import Storytime from '../data/storytime_intonations.json';

	import Scrolly from './helpers/Scrolly.svelte';

	let svg_width;
	let svg_height = 600;

	const margin = { top: 20, right: 20, bottom: 20, left: 40 };
	const width = 600 - margin.left - margin.right;
	const height = 600 - margin.top - margin.bottom;

	// chart dimensions
	$: chartWidth = svg_width - margin.left - margin.right;
	$: chartHeight = svg_height - margin.top - margin.bottom;

	function kernelDensityEstimator(kernel, X) {
		return function (V) {
			return X.map((x) => [x, d3.mean(V, (v) => kernel(x - v))]);
		};
	}

	function kernelEpanechnikov(k) {
		return function (v) {
			return Math.abs((v /= k)) <= 1 ? (0.75 * (1 - v * v)) / k : 0;
		};
	}

	$: xScale = d3.scaleLinear().domain([-10, 60]).range([0, chartWidth]);
	$: yScale = d3.scaleLinear().domain([0, 0.1]).range([chartHeight, 0]);

	$: kde = kernelDensityEstimator(kernelEpanechnikov(7), xScale.ticks(60));
	$: amazon_kde = kde(Amazon.map((d) => d.intonation_per_15));
	$: bookrecs_kde = kde(Bookrecs.map((d) => d.intonation_per_15));
	$: grwm_kde = kde(Grwm.map((d) => d.intonation_per_15));
	$: storytime_kde = kde(Storytime.map((d) => d.intonation_per_15));
	// console.log(amazon_kde);

	let currentStep;

	let steps = ['When looking at more than 300 videos across four genres of TikToks: Amazon Finds, Book Recommendations, Get Ready With Me, and Storytime...',
        '...the number of intonations in Amazon Finds TikToks were statistically significantly more than other genres.', 
        'Typically, Amazon finds TikToks had a median of about 15 intonations per 15 seconds.']
</script>

<section>
    <div class="spacer" />
    <div class="section-container">
        <div class="sticky">
            <div class="chart-space" bind:clientWidth={svg_width}>
                <svg width={svg_width} height={svg_height}>
                    <g class="chart" transform="translate({margin.left}, {margin.top})">
                        <XAxis scale={xScale} height={chartHeight} />

                        {#if currentStep === 1 || currentStep === 2}
                            <DensityLine stats={bookrecs_kde} fill_code="#D3D3D3" x={xScale} y={yScale} />
                            <DensityLine stats={grwm_kde} fill_code="#D3D3D3" x={xScale} y={yScale} />
                            <DensityLine stats={storytime_kde} fill_code="#D3D3D3" x={xScale} y={yScale} />
                            <DensityLine stats={amazon_kde} fill_code="#69b3a2" x={xScale} y={yScale} />

                            <g class="median-line" transform="translate({xScale(d3.median(Amazon.map((d) => d.intonation_per_15)))}, 0)">
                                <line y1={margin.top} y2={svg_height - margin.bottom} stroke-dasharray="5,5" stroke="#000" />
                            </g>

                        {:else}
                            <DensityLine stats={bookrecs_kde} fill_code="#404080" x={xScale} y={yScale} />
                            <DensityLine stats={grwm_kde} fill_code="#ff7f0e" x={xScale} y={yScale} />
                            <DensityLine stats={storytime_kde} fill_code="#e69c9d" x={xScale} y={yScale} />
                            <DensityLine stats={amazon_kde} fill_code="#69b3a2" x={xScale} y={yScale} />

                        {/if}
                    </g>
                </svg>
            </div>
        </div>
    
        <div class="steps">
            <Scrolly bind:value={currentStep}>
                {#each steps as step, i}
                    <!-- {console.log(currentStep)} -->
                    <div class="step" class:active={i === currentStep}>
                        <p class="step-text">{step}</p>
                    </div>
                {/each}
            </Scrolly>
        </div>
    </div>

    <div class="spacer-bottom" />
    
</section>


<style>
    .content {
        /* font-family: 'Inter-Tight', sans-serif; */
        /* font-size: 16px; */
        /* line-height: 1.5; */
        margin: auto;
        max-width: 660px;
        padding: 0 20px;
        display: flex;
        flex-direction: column;
        text-align: left;
    }

	.chart-space {
		width: 100%;
	}

    .section-container{
		display: flex;
		flex-direction: column;
		/* transition: background 400s; */
	}

    .steps {
        /* shift to the right */
        margin-left: 50%;
    }

    .step {
		display:flex;
		height: 100vh;
		text-align: center;
		margin-left: 3%;
		margin-right: 3%;
        /* background-color: aquamarine; */
	}

	.step-text{
		max-width: 50%;
		margin:auto;
		text-align: left;
		padding: 20px;
		/* border: 1px black solid; */
		z-index:99;
		background-color: #ffeaa7;
		box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
		font-size: 1.2em;
	}

    .sticky{
		height: 100%;
		position: sticky;
		top: 0;
		width: 100%;
		text-align:center;
		z-index:5;
		overflow: clip;
		
	}

    .spacer {
        height: 100px;
    }

    .spacer-bottom {
        height: 200px;
    }
</style>
