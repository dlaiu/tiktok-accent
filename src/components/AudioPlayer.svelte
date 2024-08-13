<script>
    export let src;
	export let curtainRaiser;
    import { createEventDispatcher } from 'svelte';

	import { base } from '$app/paths';

    const dispatch = createEventDispatcher();

    let time = 0;
    let duration = 0;
    let paused = true;

    function togglePlay() {
        paused = !paused;
        if (paused) {
            dispatch('pause');
        } else {
            dispatch('play');
        }
    }

    function onSeek(newTime) {
        time = newTime;
        dispatch('seek', { time });
    }

	// Dispatch currentTime whenever it updates
	$: if (!paused) {
		dispatch('timeUpdate', { currentTime: time });
    }
</script>

{#if curtainRaiser}
<div class="player">
    <audio
        src = {src}
        bind:currentTime={time}
        bind:duration
        bind:paused
        preload="metadata"
        on:play={() => dispatch('play')}
        on:pause={() => dispatch('pause')}
        on:ended={() => {
            time = 0;
            dispatch('ended');
        }}
    ></audio>
    <button class="play" aria-label={paused ? 'play' : 'pause'} on:click={togglePlay}></button>
</div>
{:else}
<div class="player">
    <audio
        src = {base + src}
        bind:currentTime={time}
        bind:duration
        bind:paused
        preload="metadata"
        on:play={() => dispatch('play')}
        on:pause={() => dispatch('pause')}
        on:ended={() => {
            time = 0;
            dispatch('ended');
        }}
    ></audio>
    <button class="play" aria-label={paused ? 'play' : 'pause'} on:click={togglePlay}></button>
</div>
{/if}


<style>
	.player {
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

	.player:not(.paused) {
		color: var(--fg-1);
		filter: drop-shadow(0.5em 0.5em 1em rgba(0,0,0,0.1));
	}
	
	button {
		width: 100%;
		aspect-ratio: 1;
		background-repeat: no-repeat;
		background-position: 50% 50%;
		border-radius: 50%;
	}
	
	[aria-label="pause"] {
		background-image: url(/component_assets/pause.svg);
	}

	[aria-label="play"] {
		background-image: url(/component_assets/play.svg);
	}

	.info {
		overflow: hidden;
	}

	.description {
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
		line-height: 1.2;
	}

	.time {
		display: flex;
		align-items: center;
		gap: 0.5em;
	}

	.time span {
		font-size: 0.7em;
	}

	.slider {
		flex: 1;
		height: 0.5em;
		background: var(--bg-2);
		border-radius: 0.5em;
		overflow: hidden;
	}

	.progress {
		width: calc(100 * var(--progress));
		height: 100%;
		background: var(--bg-3);
	}
</style>