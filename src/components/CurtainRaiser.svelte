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

	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	let recorder;
	let stream;

	let recordData;

	let audioUrl = null;
	let audio = null;
	let recording = false;
	let comparisonSection;
	let currentTime = 0; // Track the current time of the audio
	let anyaTime = 0; // Current time for Anya's audio

	// Call this function when CurtainRaiser is done
	function finishCurtainRaiser() {
        dispatch('finished');
		// console.log("triggered")
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
			console.log(audioUrl);
			console.log(audioBlob);
		});

		// document.getElementById('recordButton').innerText = 'Record Audio';
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
		console.log('Scrolling to comparison');
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
</script>

<section>
	<div class="curtainSection">
		<div class="header">
			<p>Let’s see if you sound like an influencer.</p>
			<p>Record yourself reading this typical voiceover from an Amazon influencer.</p>
		</div>
		<button
			id="recordButton"
			aria-label={!recording ? 'record' : 'pending'}
			on:click={toggleRecording}
		></button>
		<!-- <div class="button">
      
    </div> -->
		<p class="caption">"This is a breadmaker that does all the work for you."</p>
	</div>

	{#if audioUrl}
		<div class="comparison" bind:this={comparisonSection} transition:fly={{ y: 20, duration: 300, opacity: 0 }}>
			<p class="content">Now let's hear how Amazon influencer Anya Bumag sounds reading it.</p>

			<div class="hearIt">
				<p>You:</p>
				<AudioPlayer src={audioUrl} curtainRaiser=True on:timeUpdate={handleTimeUpdate} />
				<MidPointCharts data={recordData} fillColor="black" currentTime={currentTime}/>
			</div>
			<hr width="80%" />
			<div class="hearIt">
				<p>Anya:</p>
				<AudioPlayer src="/media/audio/7007951477993966853_trimmed.mp3" on:timeUpdate={handleAnyaTimeUpdate} />
				<MidPointCharts data={anyaData} fillColor="black" currentTime={anyaTime} />
			</div>

			<p class="content">What do you think is the difference here?</p>
		</div>
	{/if}

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

	[aria-label='record'] {
		background-image: url(/component_assets/record.svg);
	}

	[aria-label='pending'] {
		background-image: url(/component_assets/pending.svg);
	}

	.caption {
		font-style: italic;
		font-size: 0.9em;
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
		transition: 0.3s ease-in-out;
		width: 100vw;
		height: 80vh;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}
</style>
