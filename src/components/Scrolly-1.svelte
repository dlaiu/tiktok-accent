<script>
    import Scrolly from "./helpers/Scrolly.svelte";

    let currentStep;

    let steps = [
        // "She's not the only one that sounds this way.",
        "When we analyzed 1,500 videos from Amazon content creators, all of them had the distinctive speech pattern.",
        "Some, like myself, find this speech pattern weirdly unnatural.",
        "Adam Aleksic — a content creator who makes videos about linguistics, might call it the “TikTok accent.”"
    ]
</script>

<section>
    <div class="spacer" />

    <div class="content">
        <p style="margin-bottom:40px;">Speaking this way seems to be a feature of how TikTokers market products online.</p>
    </div>
    <div class="section-container">
            <div class="steps">
            <Scrolly bind:value={currentStep} >
                {#each steps as step, i}
                    <div class="step" class:active={i === currentStep}>
                        <p class="step-text">{step}</p>
                    </div>
                {/each}
            
            </Scrolly>
                
            </div>
            <div class="sticky">
                <div class="mosaic">
                    <div class="mosaic__item">
                        <video src="media/video/6856157769188265221.mp4" autoplay loop muted></video>
                    </div>
                    <div class="mosaic__item">
                        <video src="media/video/6957795661177818374.mp4" autoplay loop muted></video>
                    </div>
                    <div class="mosaic__item">
                        <video src="media/video/6967741777096887558.mp4" autoplay loop muted></video>
                    </div>
                    <div class="mosaic__item">
                        <video src="media/video/7231254478421282074_tobesfinds_1683657643.mp4" autoplay loop muted></video>
                    </div>
                    <div class="mosaic__item">
                        <video src="media/video/7259505958118853931_justicebuys_1690235458.mp4" autoplay loop muted></video>
                    </div>
                    <div class="mosaic__item">
                        <video src="media/video/7310344854129642757_sam.shan.shops_1702072301.mp4" autoplay loop muted></video>
                    </div>
                    <div class="mosaic__item">
                        <video src="media/video/6985308081093709062.mp4" autoplay loop muted></video>
                    </div>
                    <div class="mosaic__item">
                        <video src="media/video/6987168373041843462.mp4" autoplay loop muted></video>
                    </div>
                    <div class="mosaic__item">
                        <video src="media/video/7003842867718393093.mp4" autoplay loop muted></video>
                    </div>
                    <div class="mosaic__item">
                        <video src="media/video/7005676766522117381.mp4" autoplay loop muted></video>
                    </div>
                </div>

                
            </div>
    </div>
    <div class="spacer" />
</section>

<style>
    .mosaic {
        position: sticky;
        display: grid;
        grid-template-columns: repeat(5, auto);
        /* width: 90vw; */
        /* height: 30vh; */
        /* max-width: 800px; */
        /* max-height: 50vh; */
        margin: auto;
        overflow-x: auto; /* Allow horizontal overflow */
        background-color: grey;
        /* opacity: 80%; */
        z-index:5;
    }

    .mosaic__item {
        height:50vh;
        width: 100%;
        opacity: 60%;
    }

    .mosaic__item video {
        width: 100%;
        height: 100%;
        object-fit: cover;
    }


	.section-container{
		display: flex;
		flex-direction: column-reverse;
		/* transition: background 400s; */
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
        height: 25vh;
    }

    .content {
        /* font-family: 'Inter-Tight', sans-serif; */
        /* font-size: 16px; */
        /* line-height: 1.5; */
        margin: auto;
        width: 660px;
        max-width: 660px;
        padding: 0 20px;
        display: flex;
        flex-direction: column;
        text-align: left;
    }

    .content p {
        margin: 0.5em 0;
        font-size: 1.2em;
        line-height: 1.5;
    }

</style>