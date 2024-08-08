<script>
    import Scrolly from "./helpers/Scrolly.svelte";

    let currentStep;

    let steps = [
        "She's not the only one that sounds this way.",
        "In an analysis of six Amazon content creators and over 1,500 videos, all of them had the audibly the same speech pattern.",
        "When I first came across these videos, the speech sounded weirdly unnatural.",
        "There was something distinct about the way these Amazon Influencers spoke, but I couldn’t quite put my finger on it. "
    ]
</script>

<section>
    <div class="spacer" />
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
        background-color: aquamarine;
	}

	.step-text{
		max-width: 50%;
		margin:auto;
		text-align: left;
		padding: 20px;
		border: 1px black solid;
		z-index:99;
		background-color: white;
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

</style>