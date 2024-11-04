<script>
	import Length from "$lib/analysis/Length.svelte";
	import UniqueMelodies from "$lib/analysis/UniqueMelodies.svelte";
	import MostCommonMelodies from "$lib/analysis/MostCommonMelodies.svelte";
	import UnalteredMelodies from "$lib/analysis/UnalteredMelodies.svelte";
	import MelodyTimeline from "$lib/analysis/MelodyTimeline.svelte";
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
			color: "#7434a4",
		},
		{
			name: "Forrest's computer",
			source: forrestComputer,
			color: "#735c99",
		},
		{
			name: "Forrest's tablet",
			source: forrestTablet,
			color: "#735c99",
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

	let data = sources[0];
</script>

<section id="single-source" class="min-vh-100 d-flex flex-column align-items-center py-5">
	<div class="container">
		<h1 class="text-center mb-4">Taking a look at single sources</h1>
		<p>
			<span class="text-warning">Disclaimer about notation:</span> Some data includes a notated example which <span class="fw-bold">will not appear on smaller screens ({"<"} 992px)</span>. Also, even though a melody may be in a particular meter, the notation excludes bar lines. This is to keep the melody on a
			single line; however, the trade-off is that the rhythms may not be as clear if they were notated with measure relative ties.
		</p>
		<p>Use the dropdown to select a source to see what melodies were generated, and if/how they differ from the original.</p>
		<select class="form-select mb-3" aria-label="Choose a source" bind:value="{data}">
			<option value="" disabled>Choose a source</option>
			{#each sources as source}
				<option value="{source}">{source.name}</option>
			{/each}
		</select>
	</div>
	<div>
		{#if data}
			<Length {data} />
			<UniqueMelodies {data} />
			<MostCommonMelodies {data} />
			<UnalteredMelodies {data} />
			<MelodyTimeline {data} />
		{/if}
	</div>
</section>

<style>
	.container {
		max-width: 900px;
	}

	a {
		color: rgb(145, 248, 214);
	}

	.fw-bold {
		color: rgb(248, 203, 145);
	}
</style>
