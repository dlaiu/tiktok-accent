# Why are people talking differently online?

The idea for this project came, as one does, while scrolling on TikTok. I noticed that many influencers, particularly influencers that did Amazon product reviews. Inspired by a visual story in The Pudding about national anthems, I wanted to explain what the accent was and why these creators were speaking that way.

## Data Analysis
With the help of Professor Jonathan Soma, I scraped TikTok for its videos and metrics. I then used Parselmouth — a python implementation of the linguistic software Praat — to extract out the pitch values of every TikTok videos I was observing.

Referencing an academic paper titled The use of high rise terminals in Southern Californian English (2014) by Amanda Ritchart and Amalia Arvanti, I then implemented in python a function to track “uptalk” within the pitch values. I then counted the number of instances of uptalk there were in one video and averaged the number of instances per 15 seconds as a metric of comparison across videos and genres of TikTok.

When I plotted the density chart, I found that the median number of intonations from Amazon TikToks were visibly greater than that of other genres of TikTok. I validated this finding with a T-test in R. 

I also wanted to see if there was a correlation between this metric and the view counts of these videos. I ran a regression model but found that there was a significant relationship between the two metrics.

## Recreating the analysis
A santised version of the notebooks I used to analyse the data can be found in the code directory. A longer messier version that has all the notebooks I tried to work can be found [here](https://github.com/dlaiu/tiktok-accent-all-files). 

I am grateful for Soma's help with scraping script "running_the_scraper". It is implemented in Playwright and looks for the undocumented API while you are manually scrolling or searching through TikTok. 

The next notebook to go through is "workflow_and_analysis". There I built the dataset and cleaned it for analysis. 

In "ttest" and "running_models", I ran statistical tests on the data to validate my findings.

## Roadblocks
I faced a number of roadblocks in the process of this story. 

The first was that I did not have a background in linguistics and at the start of the project, it seemed like that would be what the project required. This lack of knowledge had me stuck for quite some time as I could not figure out and decide what would be a sound approach to analyze the data. A lot of time was spent juggling different approaches — including trying to build a machine learning classifier for what the accent is or is not — and even alternative project ideas if I had to pivot topics. Ultimately, I saw results when I understood how to codify “uptalk” and the rest of the story made a bit more sense after that. I am fortunate to have my friend Tan Yin Lin, who actually is a linguist, to teach me the basics and provide sanity checks to my analysis. 

The second was sourcing. For some reason, after reaching out to countless people, barely anybody replied. On the linguistics side, I was fortunate to have initial and background conversations with academics, to learn more about their methodologies and provide initial guidance about how to approach the analysis. However, I could not get them on the record in time for the submission of this paper.

Influencers were also unresponsive. I had reached out to creators and management companies since the start of this project and was only fortunate enough to have one reply before the deadline. She was extremely insightful and added a needed qualitative element to the story but I acknowledge that the story would have been much better with stronger reporting. 

## Visuals
Ultimately, this was a visual story, and I learnt a lot on that front. The story was built entirely in svelte and d3, with generous help from ChatGPT. 

## Future improvements
I still consider the story a work in progress. Just this week (the week of the deadline), two sources got back to me saying they would be happy to talk. 

I think the presentation and design of the story can also be greatly improved. It still doesn’t visually look how I imagined it to look, and I want to spend more time fine tuning it. There are also necessary tweaks that need to be made to the user experience of the story that I think will make a huge difference like giving options for not recording your voice and button transitions that I could not implement in time for the deadline. I hope to work on these in the coming months. 
