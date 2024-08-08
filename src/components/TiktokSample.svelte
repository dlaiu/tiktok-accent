<script>
    import WaveForm from './WaveForm.svelte';
    import AudioPlayer from './AudioPlayer.svelte';

    export let src;
    export let tiktok_src;
    export let data;
    export let transcription;
    export let caption;

    let videoElement;
    let currentTime = 0;

    function handlePlay() {
        videoElement.play();
    }

    function handlePause() {
        videoElement.pause();
    }

    function handleSeek(event) {
        videoElement.currentTime = event.detail.time;
        currentTime = event.detail.time;
    }

    // $: {
    //     console.log('Current Time:', currentTime);
    //     console.log('Transcription sample:', transcription[0]); // Check the first word's timings
    // }

    $: captionHTML = transcription.map(word => {
        const isActive = currentTime >= word['Start Time'] && currentTime < word['End Time'];
        if (isActive) {
            console.log('Active word:', word.Text); // Log active word for debugging
        }
        return `<span class="${isActive ? 'highlight' : ''}" data-start="${word['Start Time']}" data-end="${word['End Time']}" style="${isActive ? 'background-color:#dbcdf0;font-weight:bold' : ''}">${word.Text}</span>`;
    }).join(' ');

    $: formattedCaption = `<p>${captionHTML}</p>`;
</script>


<div class="tiktokSample">
    <video 
        id="tiktok-placeholder" 
        src="{tiktok_src}" 
        bind:this={videoElement}
        on:play={handlePlay}
        on:pause={handlePause}
        on:seeked={handleSeek}
        on:timeupdate={() => currentTime = videoElement.currentTime}
        muted
    />
    <div class="sound">
        <WaveForm data={data} currentTime={currentTime} />
        <div class="player">
            <AudioPlayer 
                src={src} 
                on:play={handlePlay}
                on:pause={handlePause}
                on:seek={handleSeek}
            />
            <div class="caption">
                {#if currentTime == 0}
                    <p>{caption}</p>
                {:else}
                    {@html formattedCaption}
                {/if}
            </div>
        </div>
    </div>
</div>



<style>
    .caption {
        font-style: italic;
        font-size: 0.9em;
        align-self: center;
        white-space: pre-wrap; /**Ensures text formatting and spacing is preserved**/
        margin: 10px;
    }

    #tiktok-placeholder {
        max-height: 500px;
        padding: 20px 10px;
        border-radius: 5%;
    }

    .tiktokSample {
        display: flex;
        margin: 20px 20px;
        justify-content: center;
        border: 1px solid #353535;
        border-radius: 10px;
        max-width: 65vw;
        align-self: center;
    }
    
    .sound {
        display: flex;
        flex-direction: column;
        margin: 20px 20px;
        justify-content: center;
    }

    .player {
        display: flex;
    }
</style>