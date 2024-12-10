<script>
	import Slide from "$lib/analysis/Slide.svelte";
	import MultiSource from "$lib/analysis/MultiSource.svelte";
	import InterviewTranscription from "$lib/analysis/InterviewTranscription.svelte";
	const slideTitles = [
		"Title",
		"Initial goals",
		"Melo-daily",
		"Legal Questions",
		"Starting with algorithms",
		"What is an algorithm?",
		"Recipes?",
		"Is there only one approach?",
		"Who makes algorithms?",
		"All music is algorithmic",
		"Literal music algorithms",
		"Interpretive music algorithms",
		"Law is interpretive",
		"The Fair Use Doctrine",
		"Vague language",
		"The Legal Process",
		"Where to go from here?",
		"I Could've Sworn... 🤔",
		"Step 1: input",
		"Interlude: incidentals",
		"Step 2: process",
		"Ways melodies are altered",
		"Algorithms in common",
		"Rhythm alteration",
		"Pitch alteration",
		"Truncation",
		"Step 3: output",
		"Generating a transcription",
		"Performance and interview",
		"...but we didn't do anything",
		"Why isn't there more interaction?",
		"Who is a performer?",
		"Performance doesn't mean quota",
		"Performance doesn't equal talent",
		"Which performers do we revere?",
		"What makes a performance meaningful?",
		"Accessibility is different for everyone",
		"Do performers want fulfillment?",
		"Tying this all together... 👔",
		"At the end of the day... ☀️",
	];
	let path = "/I-couldve-sworn-Ive-heard-this-somewhere-else";

	let activeLink = 0;
</script>

<main id="presentation" class="bg-dark text-light d-flex vh-100">
	<div id="presentation-toc" class="d-flex flex-column align-items-center py-2">
		<h3>Slides</h3>
		<div class="d-flex flex-column gap-2 flex-grow-1 overflow-auto w-100 ps-3">
			{#each slideTitles as title, i}
				<a
					href="#slide-{i}"
					class:active="{activeLink === i}"
					on:click="{() => {
						activeLink = i;
					}}">{title}</a>
			{/each}
		</div>
	</div>
	<div id="slides">
		<img src="{path}/slides/background.jpg" alt="background" class="position-absolute" style="width: 80%;" />
		<div class="bg position-absolute bg-dark" style="opacity: 0.9;"></div>
		<div class="position-relative w-100 h-100">
			{#each slideTitles as title, i}
				{#if i === 0}
					<div id="slide-{i}" class="slide">
						<h1 class="display-2 position-absolute start-50 top-50 translate-middle w-100 text-center">Algorithms, Access, and Ambiguity in Art and the laws surrounding Copyright</h1>
					</div>
				{:else if i === 27}
					<div id="slide-{i}" class="slide">
						<MultiSource />
					</div>
				{:else if i === 28}
					<div id="slide-{i}" class="slide">
						<InterviewTranscription />
					</div>
				{:else}
					<Slide id="{i}" {title} img="slide-{i}.png" />
				{/if}
			{/each}
		</div>
	</div>
</main>

<style>
	#presentation-toc {
		width: 20%;
		height: 100%;
		border-right: 1px solid white;
	}

	a {
		color: white;
	}

	a.active {
		color: violet;
	}

	#slides {
		width: 80%;
		height: 100%;
		overflow: hidden;
		scroll-behavior: smooth;
	}

	.bg {
		width: 80%;
		height: 100%;
	}

	.slide {
		width: 100%;
		height: 100%;
		position: relative;
		background-image: (url("/I-couldve-sworn-Ive-heard-this-somewhere-else/slides/background.jpg"));
		overflow: auto;
	}
</style>
