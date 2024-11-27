<script>
	import { base } from '$app/paths';
	import { Client } from '@gradio/client';
	import { onMount } from 'svelte';
	import WaveForm from './WaveForm.svelte';
	import AudioPlayer from './AudioPlayer.svelte';
	import MidPointCharts from './MidPointCharts.svelte';
	// import AudioPlayerCurtain from './AudioPlayerCurtain.svelte';

	import { fly } from 'svelte/transition';
	import { tick } from 'svelte';

	import anyaData from '../data/7007951477993966853_trimmed_pitch-values.json';
	import darrylData from '../data/darryl_pitch-values.json'

	import { createEventDispatcher } from 'svelte';
	import WaveFormCircles from './WaveFormCircles.svelte';

	const dispatch = createEventDispatcher();

	let recorder;
	let stream;

	let recordData;

	let audioUrl = null;
	let audio = null;
	let recording = false;
	let clicked = false;
	let disabled = false; // New state for disabling the button

	$: load = false;

	let comparisonSection;
	let currentTime = 0; // Track the current time of the audio
	let anyaTime = 0; // Current time for Anya's audio

	// Call this function when CurtainRaiser is done
	function finishCurtainRaiser() {
        // dispatch('finished');
		// console.log("triggered")

		dispatch('finished', {
	        audioUrl: audioUrl,
    	    recordData: recordData
   		});

    }

	async function startRecording() {
		stream = await navigator.mediaDevices.getUserMedia({ audio: true });
		recorder = new RecordRTC(stream, {
			type: 'audio',
			mimeType: 'audio/wav'
		});

		recorder.startRecording();
		// document.getElementById('recordButton').innerText = 'Stop Recording';
		recording = true;
		clicked = true;

		load = true;
	}

	async function stopRecording() {
		recorder.stopRecording(async () => {
			const audioBlob = recorder.getBlob();
			const tempAudioUrl = URL.createObjectURL(audioBlob);
			stream.getTracks().forEach((track) => track.stop());

			const client = await Client.connect('dlaiu/pitch-processing');
			const result = await client.predict('/predict', {
				audio_data: audioBlob
			});

			const data = JSON.parse(result.data);
			// console.log(data);

			recordData = data;
			audioUrl = tempAudioUrl;

			// recordData = {
			// 			...data,
			// 			audioBlob,
			// 			audioUrl
			// 		};
			// console.log(recordData);
			// console.log(audioUrl);
			// console.log(audioBlob);
		});

		// document.getElementById('recordButton').innerText = 'Record Audio';
		recording = false;
		clicked = false; // Reset clicked state when recording stops
		disabled = true; // Disable the button after recording stops
	}

	function toggleRecording() {
		recording = !recording;
		if (!recorder || recorder.getState() === 'inactive') {
			startRecording();
			console.log('Recording started');
		} else if (recorder.getState() === 'recording') {
			stopRecording();
			console.log('Recording ended');
		}
	}

	onMount(() => {
		if (typeof window !== 'undefined') {
			// Create a script tag
			const script = document.createElement('script');
			script.src = 'https://cdn.jsdelivr.net/npm/recordrtc@5.6.2/RecordRTC.min.js';
			script.async = true;
			script.onload = () => {
				RecordRTC = window.RecordRTC;
				// console.log('RecordRTC loaded:', RecordRTC);
			};

			// Append the script to the document
			document.body.appendChild(script);
		}

		// document.getElementById('recordButton').addEventListener('click', toggleRecording);
	});

	// Function to scroll to the comparison section
	async function scrollToComparison() {
		await tick(); // Wait for the DOM to be updated
		if (comparisonSection) {
			comparisonSection.scrollIntoView({ behavior: 'smooth' });
		}
		// console.log('Scrolling to comparison');
		finishCurtainRaiser();
	}

	$: if (audioUrl) {
		audio = new Audio(audioUrl);
		scrollToComparison();  // Scroll when audioUrl is true
		console.log(audio);

	}
	

	function handleTimeUpdate(event) {
		currentTime = event.detail.currentTime; // Update current time
	}

	function handleAnyaTimeUpdate(event) {
    	anyaTime = event.detail.currentTime;
  	}

	let played = false;
</script>

<section>
	<div class="curtainSection">
		<div class="header">
			<p class="landing">Let’s see if you sound like an influencer.</p>
			<p class="instructions">Record yourself reading this typical voiceover from an Amazon influencer.</p>
		</div>
		<button
			id="recordButton"
			aria-label={!recording ? 'record' : 'recording-active'}
			class:clicked={clicked}
			class:active={recording}
			disabled={disabled}
			on:click={toggleRecording}
			on:mouseenter={() => (hovered = true)}
			on:mouseleave={() => (hovered = false)}
		></button>
		<!-- <div class="button">
      
    </div> -->
		<p class="caption">"This is a breadmaker that does all the work for you."</p>
	</div>

	<!-- {#if load} -->
		<div class="comparison" bind:this={comparisonSection} transition:fly={{ y: 20, duration: 300, opacity: 0 }}>
			<p class="content">Now let's hear how Amazon influencer <a href="https://www.tiktok.com/@anya.bumag?lang=en">anya.bumag</a> sounds reading it.</p>

			<div class="hearIt">
				<p class="name">Anya:</p>
				<AudioPlayer src="/media/audio/7007951477993966853_trimmed.mp3" on:timeUpdate={handleAnyaTimeUpdate} on:play={() => played = true}/>
				<!-- <MidPointCharts data={anyaData} fillColor="black" currentTime={anyaTime} /> -->
				 <WaveFormCircles data={anyaData} currentTime={anyaTime} fillColor="black" curtain={true} />
			</div>
			<hr width="80%" />
			<p class="content">This is me saying the same thing.</p>
			<div class="hearIt" transition:fly={{y: 20, duration: 600, opacity: 0}}>
				<p class="name">Darryl:</p>
				<AudioPlayer src='/media/audio/darryl-curtain-audio.mp3' curtainRaiser=True on:timeUpdate={handleTimeUpdate} />
				<WaveFormCircles data={darrylData} currentTime={currentTime} fillColor="black" curtain={true}/>
			</div>

			<!-- <div class="hearIt" transition:fly={{y: 20, duration: 600, opacity: 0}}>
				<p class="name">You:</p>
				<AudioPlayer src={audioUrl} curtainRaiser=True on:timeUpdate={handleTimeUpdate} />
				<WaveFormCircles data={recordData} currentTime={currentTime} fillColor="black" curtain={true}/>
			</div> -->
			<p class="content">Notice anything different? As a hint, we've highlighted what makes Anya's voice different.</p>
			<!-- {#if played}
			<div class="hearIt" transition:fly={{y: 20, duration: 600, opacity: 0}}>
				<p class="name">You:</p>
				<AudioPlayer src={audioUrl} curtainRaiser=True on:timeUpdate={handleTimeUpdate} />
				<WaveFormCircles data={recordData} currentTime={currentTime} fillColor="black" curtain={true}/>
			</div>
			<p class="content">Notice anything different? As a hint, we've highlighted what makes Anya's voice different.</p>
			{/if} -->

		</div>
	<!-- {/if} -->

	<!-- {#if recordData}
		<div class="waveform-close-reading">
		<WaveForm data={recordData} {audioUrl} playhead="False" />
		</div>
	{/if} -->
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		gap: 1em;
		padding: 1em;
		background: var(--bg-2);
		border-radius: 1em;
	}

	.curtainSection {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		gap: 1em;
		padding: 1em;
		width: 100vw;
		height: 100vh;
	}

	.content {
		font-size: 1.2em;
		line-height: 1.5;
		font-family: 'Inter', sans-serif;
		margin: 20px;
	}

	.button {
		display: grid;
		grid-template-columns: 2.5em 1fr;
		align-items: center;
		gap: 1em;
		padding: 0.5em 1em 0.5em 0.5em;
		border-radius: 2em;
		background: var(--bg-1);
		transition: filter 0.2s;
		color: var(--fg-3);
		user-select: none;
	}

	button {
		width: 100px;
		aspect-ratio: 1;
		background-repeat: no-repeat;
		background-position: 50% 50%;
		border-radius: 50%;
		transition: background-image 0.3s ease-in-out;
	}

	/* Default Record State */
	button[aria-label='record'] {
		background-image: url(/component_assets/record.svg);
	}

	/* Record Hover State */
	button[aria-label='record']:hover {
		background-image: url(/component_assets/record.svg);
		/* Typical design choice for hover */
		transform: scale(1.05);
		box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
	}

	/* Record Clicked Hover State */
	button[aria-label='record'].clicked:hover {
		background-image: url(/component_assets/record.svg);
		box-shadow: inset 0 4px 8px rgba(0, 0, 0, 0.2);
		transform: scale(0.98); /* Slightly shrink to simulate pressing */
	}

	/* Recording Active State */
	button[aria-label='recording-active'] {
		background-image: url(/component_assets/pending.svg);
		background-color: #FF6961; 
	}

	/* Recording Active Hover State */
	button[aria-label='recording-active']:hover {
		background-image: url(/component_assets/pending.svg);
		transform: scale(1.05);
		box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
	}

	/* Recording Active Clicked Hover State */
	button[aria-label='recording-active'].clicked:hover {
		background-image: url(/component_assets/pending.svg);
		box-shadow: inset 0 4px 8px rgba(0, 0, 0, 0.2);
		transform: scale(0.98); /* Slightly shrink to simulate pressing */
	}

	/* Disabled Button State */
	button.disabled {
		background-color: #d3d3d3; /* Greyed out color */
		cursor: not-allowed;
		transform: none;
		box-shadow: none;
		opacity: 0.6;
	}

	.caption {
		font-style: italic;
		font-size: 1.2em;
		align-self: center;
	}

	.spacer {
		height: 20vh;
	}

	.hearIt {
		display: flex;
		max-width: 60vw;
		justify-content: center;
		align-items: center;
	}

	.comparison {
		
		width: 100vw;
		height: 80vh;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}
	
	.chart-overlay {
		position: relative;
		width: 100%; /* Adjust based on your desired width */
		height: 100%; /* Adjust based on your desired height */
	}

	.header {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		gap: 1em;
	}

	.landing {
		font-size: 3.5em;
		font-weight: 600;
		/* font-family: 'Inter', sans-serif; */
	}

	.instructions {
		font-size: 1.5em;
		font-weight: 400;
		/* font-family: 'Inter', sans-serif; */
	}

	.name {
		font-size: 1.5em;
		font-weight: 600;
	}
</style>
