<script>
	import { onMount } from "svelte";
	import { base } from "$app/paths";
	import { fade } from "svelte/transition";
	import Notation from "$lib/analysis/Notation.svelte";
	import aryaComputer from "$lib/analysis/Arya_computer.json";
	import aryaPhone from "$lib/analysis/Arya_phone.json";
	import emersonComputer from "$lib/analysis/Emerson_computer.json";
	import forrestComputer from "$lib/analysis/Forrest_computer.json";
	import forrestTablet from "$lib/analysis/Forrest_tablet.json";
	import kolawolePhone from "$lib/analysis/Kolawole_phone.json";
	import laurenComputer from "$lib/analysis/Lauren_computer.json";
	import michaelComputer from "$lib/analysis/Michael_computer.json";
	import nicolComputer from "$lib/analysis/Nicol_computer.json";
	import nicolPhone from "$lib/analysis/Nicol_phone.json";
	import siamakComputer from "$lib/analysis/Siamak_computer.json";
	import tylerComputer from "$lib/analysis/Tyler_computer.json";
	import tylerPhone from "$lib/analysis/Tyler_phone.json";
	import vidulaComputer from "$lib/analysis/Vidula_computer.json";

	let spokenTimestamps = [
		{
			timestamp: 3.5,
			source: "Michael",
			spoken: "We don't have anymore roles... other than, like, pressing play and listening?",
			visibility: 1,
			visible: false,
		},
		{
			timestamp: 7.5,
			source: "Forrest",
			spoken: "Yeah, I was thinking about adding an interactive element, but I just made it all... automated.",
			visibility: 1,
			visible: false,
		},
		{
			timestamp: 13.25,
			source: "Forrest",
			spoken: "And I made a little visualizer for it, as well, so you have something to stare at for 5 minutes while we're all quiet... so.",
			visibility: 1,
			visible: false,
		},
		{
			timestamp: 13.75,
			source: "Michael",
			spoken: "😂😂👃😂",
			visibility: 0.75,
			visible: false,
		},
		{
			timestamp: 16.5,
			source: "Nicol",
			spoken: "😂",
			visibility: 0.8,
			visible: false,
		},
		{
			timestamp: 18,
			source: "Nicol",
			spoken: "That's great.",
			visibility: 0.8,
			visible: false,
		},
		{
			timestamp: 18.75,
			source: "Forrest",
			spoken: "Yeah",
			visibility: 0.6,
			visible: false,
		},
		{
			timestamp: 20.9,
			source: "Nicol",
			spoken: "Okay...",
			visibility: 0.8,
			visible: false,
		},
		{
			timestamp: 21,
			source: "Michael",
			spoken: "Lordy...",
			visibility: 0.8,
			visible: false,
		},
		{
			timestamp: 21.45,
			source: "Nicol",
			spoken: "I think we're gonna go ahead... if yours is still... working on loading, just hit play when it has loaded.",
			visibility: 0.8,
			visible: false,
		},
		{
			timestamp: 27.3,
			source: "Vahid",
			spoken: "Yeah, it's just loading, so...",
			visibility: 0.4,
			visible: false,
		},
		{
			timestamp: 27.85,
			source: "Nicol",
			spoken: "Yeah",
			visibility: 0.8,
			visible: false,
		},
		{
			timestamp: 28.3,
			source: "Nicol",
			spoken: "When... when it's ready, hit play, so let's... begin with what we've got.",
			visibility: 0.8,
			visible: false,
		},
		{
			timestamp: 28.35,
			source: "Unknown",
			spoken: "???",
			visibility: 0.8,
			visible: false,
		},
		{
			timestamp: 32.45,
			source: "Siamak",
			spoken: "Please, ???",
			visibility: 0.3,
			visible: false,
		},
		{
			timestamp: 32.5,
			source: "Forrest",
			spoken: "Just gonna make sure I'm recording real quick.",
			visibility: 1,
			visible: false,
		},
		{
			timestamp: 33.7,
			source: "Michael",
			spoken: "You need it coordinated, or...?",
			visibility: 0.7,
			visible: false,
		},
		{
			timestamp: 35.15,
			source: "Forrest",
			spoken: "Yeah, I'll count down from 5, and when I say go... we'll all hit the play button at the same time.",
			visibility: 1,
			visible: false,
		},
		{
			timestamp: 39.4,
			source: "Forrest",
			spoken: "...or, it doesn't matter that it's exact... we can all be within a couple of seconds of one another, if your computer has a hiccup, or whatever",
			visibility: 1,
			visible: false,
		},
		{
			timestamp: 41.75,
			source: "Michael",
			spoken: "😆",
			visibility: 0.7,
			visible: false,
		},
		{
			timestamp: 43.3,
			source: "Tyler",
			spoken: "Okay.",
			visibility: 0.7,
			visible: false,
		},
		{
			timestamp: 45.45,
			source: "Tyler",
			spoken: "Ooh, should I turn the volume up?",
			visibility: 0.5,
			visible: false,
		},
		{
			timestamp: 46.75,
			source: "Forrest",
			spoken: "Yeah... yeah, crank it.",
			visibility: 1,
			visible: false,
		},
		{
			timestamp: 49.25,
			source: "Forrest",
			spoken: "Okay, uh...",
			visibility: 1,
			visible: false,
		},
		{
			timestamp: 51.25,
			source: "Forrest",
			spoken: "Any questions about anything?",
			visibility: 1,
			visible: false,
		},
		{
			timestamp: 52.5,
			source: "Vidula",
			spoken: "Hm-mm",
			visibility: 0.4,
			visible: false,
		},
		{
			timestamp: 53.25,
			source: "Forrest",
			spoken: "We're good...? Alright.",
			visibility: 1,
			visible: false,
		},
		{
			timestamp: 54.25,
			source: "Forrest",
			spoken: "5.",
			visibility: 1,
			visible: false,
		},
		{
			timestamp: 55.25,
			source: "Forrest",
			spoken: "4.",
			visibility: 1,
			visible: false,
		},
		{
			timestamp: 56.25,
			source: "Forrest",
			spoken: "3.",
			visibility: 1,
			visible: false,
		},
		{
			timestamp: 57.25,
			source: "Forrest",
			spoken: "2.",
			visibility: 1,
			visible: false,
		},
		{
			timestamp: 58.25,
			source: "Forrest",
			spoken: "1.",
			visibility: 1,
			visible: false,
		},
		{
			timestamp: 59.25,
			source: "Forrest",
			spoken: "Go.",
			visibility: 1,
			visible: false,
		},
		{
			timestamp: 355.45,
			source: "Forrest",
			spoken: "???",
			visibility: 1,
			visible: false,
		},
		{
			timestamp: 357.45,
			source: "Forrest",
			spoken: "???",
			visibility: 1,
			visible: false,
		},
		{
			timestamp: 375.25,
			source: "Forrest",
			spoken: "Awe, that was pretty satisfying.",
			visibility: 1,
			visible: false,
		},
		{
			timestamp: 376,
			source: "Michael",
			spoken: "Alright!",
			visibility: 1,
			visible: false,
		},
		{
			timestamp: 376.75,
			source: "Michael",
			spoken: "😆😆😆",
			visibility: 1,
			visible: false,
		},
		{
			timestamp: 377,
			source: "Nicol",
			spoken: "😆",
			visibility: 1,
			visible: false,
		},
		{
			timestamp: 378.25,
			source: "Forrest",
			spoken: "It worked!",
			visibility: 1,
			visible: false,
		},
		{
			timestamp: 379.25,
			source: "Nicol",
			spoken: "Very cool!",
			visibility: 1,
			visible: false,
		},
	];
	let playing = false;
	let audio, loaded, melodyTimestamps, spoken, duration, currentTime;

	const sources = [
		{
			name: "Arya's computer",
			source: aryaComputer,
		},
		{
			name: "Arya's phone",
			source: aryaPhone,
		},
		{
			name: "Emerson's computer",
			source: emersonComputer,
			color: "#800080",
		},
		{
			name: "Forrest's computer",
			source: forrestComputer,
			color: "#E6E6FA",
		},
		{
			name: "Forrest's tablet",
			source: forrestTablet,
			color: "#E6E6FA",
		},
		{
			name: "Kolawole's phone",
			source: kolawolePhone,
		},
		{
			name: "Lauren's computer",
			source: laurenComputer,
		},
		{
			name: "Michael's computer",
			source: michaelComputer,
		},
		{
			name: "Nicol's computer",
			source: nicolComputer,
			color: "#FF69B4",
		},
		{
			name: "Nicol's phone",
			source: nicolPhone,
			color: "#FF69B4",
		},
		{
			name: "Siamak's computer",
			source: siamakComputer,
		},
		{
			name: "Tyler's computer",
			source: tylerComputer,
		},
		{
			name: "Tyler's phone",
			source: tylerPhone,
		},
		{
			name: "Vidula's computer",
			source: vidulaComputer,
		},
	];

	function stripTime(time) {
		let t = time.split(",");

		if (time.includes("पू")) {
			return `${t[1].substring(4)}`;
		} else if (time.includes("AM")) {
			return `${t[1].substring(1, t[1].length - 3)}`;
		} else {
			return `${t[1].substring(1)}`;
		}
	}

	function melodyDuration(melody) {
		const tempo = melody.alteredTempo;
		let duration = 0;

		for (const note of melody.alteredMelody) {
			switch (note[1]) {
				case "2n":
					duration += 2;
					break;
				case "4n":
					duration += 1;
					break;
				case "8n":
					duration += 0.5;
					break;
				case "16n":
					duration += 0.25;
					break;
				case "2n.":
					duration += 3;
					break;
				case "4n.":
					duration += 1.5;
					break;
				case "8n.":
					duration += 0.75;
					break;
				case "16n.":
					duration += 0.375;
					break;
				case "2n..":
					duration += 3.5;
					break;
				case "4n..":
					duration += 1.75;
					break;
				case "8n..":
					duration += 0.875;
					break;
				case "16n..":
					duration += 0.4375;
					break;
				default:
					break;
			}
		}

		return (duration / tempo) * 60;
	}

	function convertToSeconds(time) {
		return time.reduce((acc, curr, i) => {
			if (i === 0) {
				return acc + parseInt(curr) * 3600;
			} else if (i === 1) {
				return acc + parseInt(curr) * 60;
			} else {
				return acc + parseInt(curr);
			}
		}, 0);
	}

	function findMelodiesAtGivenTime(time, range) {
		for (const source of sources) {
			for (const melody of source.source) {
				const sourceTime = convertToSeconds(time.split(":"));
				const timestamp = convertToSeconds(stripTime(melody.timestamp).split(":"));

				if (timestamp >= sourceTime - range && timestamp <= sourceTime + range) {
					console.log(source.name, melody);
				}
			}
		}
	}

	function populateMelodyTimestamps() {
		const startTime = convertToSeconds("10:14:45".split(":"));
		const endTime = convertToSeconds("10:21:04".split(":"));
		const arr = [];
		let visibility;

		for (const source of sources) {
			for (const melody of source.source) {
				const timestamp = convertToSeconds(stripTime(melody.timestamp).split(":"));

				if (source.name.includes("Forrest") || source.name.includes("Tyler") || source.name.includes("Michael") || source.name.includes("Emerson")) {
					visibility = 1;
				} else if (source.name.includes("Nicol") || source.name.includes("Vidula")) {
					visibility = 0.75;
				} else if (source.name.includes("Siamak") || source.name.includes("Kolawole")) {
					visibility = 0.5;
				} else {
					visibility = 0.25;
				}

				if (timestamp >= startTime && timestamp <= endTime) {
					arr.push({
						timestamp: timestamp - startTime,
						duration: melodyDuration(melody),
						source: source.name,
						melody: melody,
						visible: false,
						visibility,
					});
				}
			}
		}

		melodyTimestamps = arr.sort((a, b) => a.timestamp - b.timestamp);
	}

	function start() {
		audio.play();
		playing = true;
	}

	function scrollToBottom() {
		if (spoken) {
			spoken.scrollTop = spoken.scrollHeight;
		}
	}

	onMount(() => {
		duration = 381;
		currentTime = 0;

		audio = new Audio(`${base}/performance.mp3`);
		audio.addEventListener("loadeddata", () => {
			loaded = true;
			populateMelodyTimestamps();
		});

		audio.addEventListener("timeupdate", () => {
			let updatedMelodies = melodyTimestamps.map((melody) => {
				if (audio.currentTime >= melody.timestamp && audio.currentTime <= melody.timestamp + melody.duration) {
					return { ...melody, visible: true };
				} else {
					return { ...melody, visible: false };
				}
			});
			melodyTimestamps = updatedMelodies;

			duration = audio.duration;
			currentTime = audio.currentTime;

			let updatedSpoken = spokenTimestamps.map((spoken) => {
				if (audio.currentTime >= spoken.timestamp) {
					return { ...spoken, visible: true };
				} else {
					return { ...spoken, visible: false };
				}
			});
			spokenTimestamps = updatedSpoken;

			scrollToBottom();
		});
	});
</script>

<section id="transcription" class="min-vh-100 d-flex flex-column justify-content-between py-5 text-light">
	<div class="container">
		<h1 class="text-center">Field recording transcription</h1>
		<h3 class="text-center">Performance</h3>
		<p>
			The transcription/visualization below is of a field recording of a performance of <span class="fst-italic">I could've sworn I've heard this somewhere else... 🤔.</span> The recording is <span class="fw-bold">6 minutes and 20 seconds long</span>, and based on
			<a href="#timings">how I had to figure out specific timings in the recording</a>, this selection starts <span class="fw-bold">4 minutes and 5 seconds</span> into the original recording. That means its starting time can be <span class="fw-bold">anywhere between 10:14:45 and 10:15:04</span>.
		</p>
		<p>The transcription consists of both melodic and spoken elements. The visibility of the transcribed element corresponds with how audible it is in the recording.</p>
	</div>
	<div class="container flex-grow-1 {playing ? 'position-relative' : 'd-flex justify-content-center align-items-center'}">
		{#if !playing}
			{#if loaded}
				<button class="btn btn-light" on:click="{start}">Play</button>
			{:else}
				<div class="spinner-border" role="status">
					<span class="visually-hidden">Loading...</span>
				</div>
			{/if}
		{:else if duration && currentTime}
			<input type="range" class="form-range" min="0" max="{duration}" bind:value="{currentTime}" on:input="{() => (audio.currentTime = currentTime)}" />
			<div class="d-flex flex-column gap-4" transition:fade>
				<div bind:this="{spoken}" class="d-flex flex-column gap-2 overflow-y-auto" style="height: 400px;">
					<h4>Spoken</h4>
					{#each spokenTimestamps as { timestamp, duration, source, spoken, visibility, visible }}
						{#if visibility > 0 && visible}
							<div class="text-light" style="opacity: {visibility};" transition:fade>
								<p class="m-0">{source}</p>
								<p class="m-0">"{spoken}"</p>
							</div>
						{/if}
					{/each}
				</div>
				<div>
					<h4>Melodies</h4>
					<div class="d-flex gap-3 flex-wrap">
						{#each melodyTimestamps as { timestamp, duration, source, melody, visible, visibility }}
							{#if visible && visibility > 0}
								<div class="text-light" style="opacity: {visibility};" transition:fade>
									<p class="m-0">{source}</p>
									<p class="m-0">{stripTime(melody.timestamp)}</p>
									<p class="m-0 fst-italic">{melody.melodyName}</p>
								</div>
							{/if}
						{/each}
					</div>
				</div>
			</div>
		{/if}
	</div>
</section>

<style>
	.container {
		max-width: 800px;
	}

	a {
		color: rgb(145, 248, 214);
	}

	.fw-bold {
		color: rgb(248, 203, 145);
	}

	/*********** Baseline, reset styles ***********/
	input[type="range"] {
		-webkit-appearance: none;
		appearance: none;
		background: transparent;
		cursor: pointer;
		width: 100%;
	}

	/* Removes default focus */
	input[type="range"]:focus {
		outline: none;
	}

	/******** Chrome, Safari, Opera and Edge Chromium styles ********/
	/* slider track */
	input[type="range"]::-webkit-slider-runnable-track {
		background-color: #000000;
		border-radius: 5px;
		height: 1rem;
	}

	/* slider thumb */
	input[type="range"]::-webkit-slider-thumb {
		-webkit-appearance: none; /* Override default look */
		appearance: none;
		margin-top: 0px; /* Centers thumb on the track */
		background-color: #b781cf;
		border-radius: 5px;
		height: 1rem;
		width: 1rem;
	}

	input[type="range"]:focus::-webkit-slider-thumb {
		outline: 3px solid #b781cf;
		outline-offset: 0.125rem;
	}

	/*********** Firefox styles ***********/
	/* slider track */
	input[type="range"]::-moz-range-track {
		background-color: #000000;
		border-radius: 5px;
		height: 1rem;
	}

	/* slider thumb */
	input[type="range"]::-moz-range-thumb {
		background-color: #b781cf;
		border: none; /*Removes extra border that FF applies*/
		border-radius: 5px;
		height: 1rem;
		width: 1rem;
	}

	input[type="range"]:focus::-moz-range-thumb {
		outline: 3px solid #b781cf;
		outline-offset: 0.125rem;
	}
</style>
