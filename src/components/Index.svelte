<script>
    import { base } from '$app/paths';

	import WaveForm from "./WaveForm.svelte";
    import data from "../data/7366018710248672517_pitch_values.json"
	import AudioPlayer from "./AudioPlayer.svelte";
	import TiktokSample from "./TiktokSample.svelte";
	import Scrolly_1 from "./Scrolly-1.svelte";
	import WaveFormAnnotated from "./WaveFormAnnotated.svelte";
	import DensityChart from "./DensityChart.svelte";
    import WaveFormAnnotatedCircles from "./WaveFormAnnotatedCircles.svelte";

    import transcript from '../data/7366018710248672517_transcription.json'
	import WaveFormPlayer from "./WaveFormPlayer.svelte";
	import CurtainRaiser from './CurtainRaiser.svelte';
	import { html } from 'd3';
	import Footer from './Footer.svelte';
    
    let curtainRaiserDone = false;

    function handleCurtainRaiserFinished() {
        curtainRaiserDone = true;
        console.log("curtain raiser is done.")
    }
    
    let currentTimeAnnotated = 0;
    
    function handleTimeUpdate(event) {
        currentTimeAnnotated = event.detail.currentTime;
    }

</script>
<body>
    <div class="curtainRaiser">
        <CurtainRaiser on:finished={handleCurtainRaiserFinished}/>
    </div>


    
    {#if curtainRaiserDone}
    <div class="hero">
        <p class="headline">Why are people talking differently online?</p>
        <p class="byline">By Darryl Laiu</p>
    </div>

    <div class="content">
        <p>Bumag (whose real name is Anya Bumagin) says that her audience has noticed the peculiar way she speaks in her videos.</p>
        <p>But Bumagin is not the only one that speaks this way.</p>
        <p>Here's a video of another Amazon influencer, <a href="https://www.tiktok.com/@sam.shan.shops?lang=en">sam.shan.shops</a>. Pay close attention to what is happening at the parts of the audio marked by the circles.</p>
    </div>  
    <TiktokSample 
        data={data} 
        src='/media/audio/7366018710248672517.mp3' 
        caption="“Number one, Amazon new releases. This water pitcher has an automatic touch dispenser and is convenient to store in your fridge any room in your home or on the go. It comes in two different sizes and filters your water clean of microplastics, mercury and so much more.”"
        tiktok_src="/media/video/7366018710248672517.mp4"
        transcription={transcript} 
    />

    <!-- <div class="content">
        <p>Speaking this way seems to be a feature of how TikTokers market products online.</p>
    </div> -->

    <Scrolly_1 />

    <div class="content">

        <p class="subhead" style="font-size: 2em;font-weight: bold;font-family:'Rasa', 'serif';">Attention and engagement</p>

        <p>Aleksic said in an explainer of the accent that it "sounds like that because it's designed to manipulate you."</p>
       
        <p>There's something distinct about the way these influencers speak that's hard to define. <a href="https://www.npr.org/2024/01/31/1228067629/exploring-how-content-creators-speak-on-tiktok-it-s-known-as-influencer-speak" target="_blank">According to a report by NPR</a>, this accent is marked by two key features: <span class="bold">vocal fry</span> and <span class="bold">upspeak.</span></p>

        <p><span class="bold">Vocal fry</span> is when the speech sounds more gravelly. </p>

        <!-- <blockquote class="tiktok-embed" cite="https://www.tiktok.com/@beccajacksonstudio/video/7348952911289814302" data-video-id="7348952911289814302" style="max-width: 605px;min-width: 325px;" > <section> <a target="_blank" title="@beccajacksonstudio" href="https://www.tiktok.com/@beccajacksonstudio?refer=embed">@beccajacksonstudio</a> There are many misconceptions about vocal fry.  It’s one of my favorite subjects to get deep into.   Here it is in less than a minute so the next time your choir director or acting teacher mentions it you know what they are talking about! Gotta know the basics before you dive in!  <a title="musicaltheatre" target="_blank" href="https://www.tiktok.com/tag/musicaltheatre?refer=embed">#musicaltheatre</a> <a title="actingteacher" target="_blank" href="https://www.tiktok.com/tag/actingteacher?refer=embed">#actingteacher</a> <a title="voiceteacher" target="_blank" href="https://www.tiktok.com/tag/voiceteacher?refer=embed">#voiceteacher</a> <a title="voicecoach" target="_blank" href="https://www.tiktok.com/tag/voicecoach?refer=embed">#voicecoach</a> <a title="actingtips" target="_blank" href="https://www.tiktok.com/tag/actingtips?refer=embed">#actingtips</a> <a title="education" target="_blank" href="https://www.tiktok.com/tag/education?refer=embed">#education</a> <a title="onlinecoach" target="_blank" href="https://www.tiktok.com/tag/onlinecoach?refer=embed">#onlinecoach</a> <a title="actinglessons" target="_blank" href="https://www.tiktok.com/tag/actinglessons?refer=embed">#actinglessons</a> <a title="singingteacher" target="_blank" href="https://www.tiktok.com/tag/singingteacher?refer=embed">#singingteacher</a> <a title="voicelessons" target="_blank" href="https://www.tiktok.com/tag/voicelessons?refer=embed">#voicelessons</a> <a title="singing" target="_blank" href="https://www.tiktok.com/tag/singing?refer=embed">#singing</a> <a title="singingtips" target="_blank" href="https://www.tiktok.com/tag/singingtips?refer=embed">#singingtips</a> <a title="vocalcoach" target="_blank" href="https://www.tiktok.com/tag/vocalcoach?refer=embed">#vocalcoach</a> <a title="fyp" target="_blank" href="https://www.tiktok.com/tag/fyp?refer=embed">#fyp</a> <a title="careeradvice" target="_blank" href="https://www.tiktok.com/tag/careeradvice?refer=embed">#careeradvice</a> <a target="_blank" title="♬ original sound - Becca Jackson Studio" href="https://www.tiktok.com/music/original-sound-7348952951752280863?refer=embed">♬ original sound - Becca Jackson Studio</a> </section> </blockquote> <script async src="https://www.tiktok.com/embed.js"></script> -->

        <!-- {@html `<blockquote class="tiktok-embed" cite="https://www.tiktok.com/@beccajacksonstudio/video/7348952911289814302" data-video-id="7348952911289814302" style="max-width: 605px;min-width: 325px;" > <section> <a target="_blank" title="@beccajacksonstudio" href="https://www.tiktok.com/@beccajacksonstudio?refer=embed">@beccajacksonstudio</a> There are many misconceptions about vocal fry.  It’s one of my favorite subjects to get deep into.   Here it is in less than a minute so the next time your choir director or acting teacher mentions it you know what they are talking about! Gotta know the basics before you dive in!  <a title="musicaltheatre" target="_blank" href="https://www.tiktok.com/tag/musicaltheatre?refer=embed">#musicaltheatre</a> <a title="actingteacher" target="_blank" href="https://www.tiktok.com/tag/actingteacher?refer=embed">#actingteacher</a> <a title="voiceteacher" target="_blank" href="https://www.tiktok.com/tag/voiceteacher?refer=embed">#voiceteacher</a> <a title="voicecoach" target="_blank" href="https://www.tiktok.com/tag/voicecoach?refer=embed">#voicecoach</a> <a title="actingtips" target="_blank" href="https://www.tiktok.com/tag/actingtips?refer=embed">#actingtips</a> <a title="education" target="_blank" href="https://www.tiktok.com/tag/education?refer=embed">#education</a> <a title="onlinecoach" target="_blank" href="https://www.tiktok.com/tag/onlinecoach?refer=embed">#onlinecoach</a> <a title="actinglessons" target="_blank" href="https://www.tiktok.com/tag/actinglessons?refer=embed">#actinglessons</a> <a title="singingteacher" target="_blank" href="https://www.tiktok.com/tag/singingteacher?refer=embed">#singingteacher</a> <a title="voicelessons" target="_blank" href="https://www.tiktok.com/tag/voicelessons?refer=embed">#voicelessons</a> <a title="singing" target="_blank" href="https://www.tiktok.com/tag/singing?refer=embed">#singing</a> <a title="singingtips" target="_blank" href="https://www.tiktok.com/tag/singingtips?refer=embed">#singingtips</a> <a title="vocalcoach" target="_blank" href="https://www.tiktok.com/tag/vocalcoach?refer=embed">#vocalcoach</a> <a title="fyp" target="_blank" href="https://www.tiktok.com/tag/fyp?refer=embed">#fyp</a> <a title="careeradvice" target="_blank" href="https://www.tiktok.com/tag/careeradvice?refer=embed">#careeradvice</a> <a target="_blank" title="♬ original sound - Becca Jackson Studio" href="https://www.tiktok.com/music/original-sound-7348952951752280863?refer=embed">♬ original sound - Becca Jackson Studio</a> </section> </blockquote> <script async src="https://www.tiktok.com/embed.js"></script>`} -->

        <video src="/media/video/tiktok_1.mp4" autoplay muted loop controls style="max-height:400px;"></video>

        <p><span class="bold">Upspeak</span> is when you end sentences the same way you end questions.</p>

        <p>Aleksic (who goes by Etymologynerd on TikTok) sees uspeak as a key feature of the TikTok voice.</p>

        <p>“The staccato uptick in your voice when you’re talking keeps people watching for three main reasons,” says Aleksic.</p>
    </div>

    <div class="tiktok-quote">
        <!-- {@html

        `<blockquote class="tiktok-embed" cite="https://www.tiktok.com/@etymologynerd/video/7307378701140954414" data-video-id="7307378701140954414" style="max-width: 605px;min-width: 325px;" > <section> <a target="_blank" title="@etymologynerd" href="https://www.tiktok.com/@etymologynerd?refer=embed">@etymologynerd</a> Unusual methods of phonation, like glottal fry or breathiness, also help retain viewers, and I could do an entire follow-up video on that The uptick is called &#34;High Rising Terminal,&#34; or HRT, and I recently did a video on how it&#39;s used in frat bro sociolects, which you can see here:  @etymologynerd  I&#39;m very aware that I do this too, and at this point it feels more natural than talking normally to the camera. I&#39;ve been wanting to make this video for a while so I could call attention to the phenomenon <a title="linguistics" target="_blank" href="https://www.tiktok.com/tag/linguistics?refer=embed">#linguistics</a> <a title="dialect" target="_blank" href="https://www.tiktok.com/tag/dialect?refer=embed">#dialect</a> <a title="language" target="_blank" href="https://www.tiktok.com/tag/language?refer=embed">#language</a> <a title="sociology" target="_blank" href="https://www.tiktok.com/tag/sociology?refer=embed">#sociology</a> <a title="influenceraccent" target="_blank" href="https://www.tiktok.com/tag/influenceraccent?refer=embed">#influenceraccent</a> <a target="_blank" title="♬ original sound - etymologynerd" href="https://www.tiktok.com/music/original-sound-7307378759529663278?refer=embed">♬ original sound - etymologynerd</a> </section> </blockquote> <script async src="https://www.tiktok.com/embed.js"></script>`

        } -->

        <video src="/media/video/tiktok_2.mp4" autoplay muted loop controls style="max-height:400px;"></video>

        <div class="blockquote">
            <p>“First, the rising tones makes it sound like the video is always unfinished, so people have to keep watching because they can tell that something is coming next. </p>
            <p>Second is they’re seeking validation from you all the time, so you as the viewer feel like you’re more actively participating in the video, which makes you more engaged with it, and also you just feel more comfortable with the informal vibe.</p>
            <p>Third, it’s metalinguistic signaling, communicating social information. It’s telling people I’m a certain kind of person. I’m an influencer. And then that frames your perception of how much you think you’re going to enjoy the video.“</p>
        </div>
    </div>

    <div class="content">
        <p>Engagement seems to be the primary speculation for why influencers speak in that particular way.</p>

        <p>However, an analysis on the frequency of uptalk in a video and the number of views a video has showed that there wasn't a significant relationship between the two.</p>

        <p class="subhead" style="font-size: 2em;font-weight: bold;font-family:'Rasa', 'serif';">Emphasis</p>

        <p>Bumagin says that she does intentionally speak in a lower voice. However, she said that she doesn't uptalk intentionally.</p>

        <p>"I think it's natural...in that context to emphasise certain things."</p>

        <p class="subhead" style="font-size: 2em;font-weight: bold;font-family:'Rasa', 'serif';">Uptalk</p>

        <p>For a moment, let's look at <span class="bold">upspeak.</span></p>

        <p>Listening to the clip again, let's highlight every time the pitch of their voice goes up.</p>

        <div class="sound">
            <WaveFormPlayer 
                data={data} 
                src='/media/audio/7366018710248672517.mp3'
                transcription={transcript}
                caption="“Number one, Amazon new releases. This water pitcher has an automatic touch dispenser and is convenient to store in your fridge any room in your home or on the go. It comes in two different sizes and filters your water clean of microplastics, mercury and so much more.”"  
            />
        </div>

        <p>Notice how the uptalk stands out.</p> 

        <p>Compared to other genres of TikTok, it almost sounds like they’re overdoing the uptalk.</p>

        
    </div>

    <DensityChart />

    <div class="content">
        <p>Consider this 15s clip,</p>

        <div class="sound-close-reading">
            <WaveFormAnnotatedCircles data={data} currentTime={currentTimeAnnotated}/>
            <div class="player">
                <AudioPlayer src='/media/audio/7366018710248672517.mp3' on:timeUpdate={handleTimeUpdate} />
                <p class="caption">"“<span class="highlight">Number</span> <span class="highlight">one</span>, <span class="highlight">Amazon</span> new releases. <span class="highlight">This</span> <span class="highlight">water</span> <span class="highlight">pitcher</span> has an <span class="highlight">automatic</span> <span class="highlight">touch</span> dispenser and is <span class="highlight">convenient</span> to <span class="highlight">store</span> in your <span class="highlight">fridge</span> any <span class="highlight">room</span> in your home or on the go. It comes in two <span class="highlight">different</span> <span class="highlight">sizes</span> and filters your water clean of <span class="highlight">microplastics,</span> mercury and so much more.”"</p>
            </div>
        </div>

        <p>we can see that words like "Amazon", the product introduced, and its features, are emphasised.</p>
    </div>

    <div class="content">
        <p>Rising intonations aren't just useful for convicing people.</p>
        <p><a href="https://www.sciencedirect.com/science/article/pii/S037821661730663X">In a 2018 paper by Burdin and Tyler,</a> they found that this rise in intonation tends to be found when people list things off as well.</p>
        <p>In their study, they found that rises were associated with feelings of helpfulness, niceness, and friendliness towards the speaker. </p>
        <p>The speaker also comes across as more prepared about what they are saying, and the speaker that used “rises” was heard as “informing” and “instructive”.</p>
        <p class="subhead" style="font-size: 2em;font-weight: bold;font-family:'Rasa', 'serif';">Trends & Emulation</p>
        <p>Bumagin said that the her intonations are just part of the natural way she speaks "to emphasize certain things".</p>
        <p>In my conversation her, I could hear those same intonations. However, they sounded more organic because it was peppered through sentences of varying pitch as opposed to grouped together, sentence after sentence.</p>
        <p>Her manager, Andreea Ozturk, who was in the call with us, said that much like trends, these accents could have started as something fun that ends up being copied or emulated by other people.</p>
        <p>"Let's say tomorrow, Anya creates a video...in a crazy, totally different way," said Ozturk. "Other creators may copy her [the way she did it]."</p> 
        <p>When I asked Bumagin about how she think she's different from other Amazon influencers, Bumagin said that despite everything we've talked about, she feels that everyone has a different voice.</p>
        <p>"Everyone has a different voice and a different way of speaking," said Bumagin. "Some followers might relate to one creator more [that the other because of] the way they speak as well."</p>
    </div>

    <Footer />
    {/if}

</body>

<style>
    body {
        display: flex;
        flex-direction: column;
        justify-content: center;
        font-family: 'Inter-Tight', sans-serif;
        font-style: normal;
        /* font-size: 1em; */
        /* line-height: 1.5; */
        margin: 0;
        padding: 0;
        background-color: #fdecde;
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

    .hero {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        margin: 50px 0;
        width: 100vw;
        height: 60vh;
    }

    .headline {
        font-size: 5em;
        font-weight: bold;
        font-family: 'Rasa', serif;

    }

    .byline {
        font-size: 1.5em;
        font-family: 'Rasa', serif;
    }

    .content p {
        margin: 0.5em 0;
        font-size: 1.2em;
        line-height: 1.5;
    }

    .bold {
        font-weight: bold;
    }

    /* .curtainRaiser {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
    } */

    .tiktok-quote {
        display: flex;
        margin: 20px 0;
        justify-content: center;
        align-items: center;
        max-width: 800px;
        align-self: center;
    }
    
    .blockquote {
        margin-left: 20px;
        font-style: italic;
        line-height: 1.5;
        font-family: 'Rasa', sans-serif;
        font-size: 1.3em;   
        position: relative;
        padding-left: 20px;
        border-left: 2px solid black; /* Add this line */
    }

    .blockquote p {
        margin: 0.5em 0;
    }   

    .sound {
        display: flex;
        flex-direction: column;
        margin: 20px 20px;
        justify-content: center;
    }

    .sound-close-reading {
        display: flex;
        flex-direction: column;
        margin: 20px 20px;
        justify-content: center;
        width: 100%;
    }

    .player {
        display: flex;
        align-items: center;
        width:auto;
    }

    .waveform-close-reading {
        display: flex;
        justify-content: center;
        /* fit the width to parent width */
        width: 100%;

    }
    
    .player .caption {
        font-style: italic;
        font-size: 0.9em;
        align-self: center;
    }

    /* .density-chart {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 80vh;
    } */

    .highlight {
        background-color: #dbcdf0;
        padding: 2px;
        border-radius: 5px;
    }
</style>
