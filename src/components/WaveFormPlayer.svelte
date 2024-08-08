<script>
    export let src;
    export let data;
    export let transcription;
    export let caption;

    import WaveFormAnnotated from './WaveFormAnnotated.svelte';
    import AudioPlayer from './AudioPlayer.svelte';

    let currentTime = 0;

    function updateTime(event) {
        currentTime = event.detail.currentTime;
    }

    $: captionHTML = transcription.map(word => {
        const isActive = currentTime >= word['Start Time'] && currentTime < word['End Time'];
        if (isActive) {
            console.log('Active word:', word.Text); // Log active word for debugging
        }
        return `<span class="${isActive ? 'highlight' : ''}" data-start="${word['Start Time']}" data-end="${word['End Time']}" style="${isActive ? 'background-color:#dbcdf0;font-weight:bold' : ''}">${word.Text}</span>`;
    }).join(' ');

    $: formattedCaption = `<p>${captionHTML}</p>`;
</script>

<WaveFormAnnotated data={data} currentTime={currentTime}/>
<div class="player">
    <AudioPlayer src='/media/audio/7366018710248672517.mp3' on:timeUpdate={updateTime}/>
    <!-- <p class="caption">“Number one, Amazon new releases. This water pitcher has an automatic touch dispenser and is convenient to store in your fridge any room in your home or on the go. It comes in two different sizes and filters your water clean of microplastics, mercury and so much more.”</p> -->

    <div class="caption">
        {#if currentTime == 0}
            <p>{caption}</p>
        {:else}
            {@html formattedCaption}
        {/if}
    </div>
</div>

<style>
    .player {
        display: flex;
        align-items: center;
        width:auto;
    }

    .caption {
        font-style: italic;
        font-size: 0.9em;
        align-self: center;
    }
</style>