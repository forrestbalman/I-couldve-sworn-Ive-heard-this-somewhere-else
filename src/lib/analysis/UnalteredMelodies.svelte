<script>
	import Notation from "$lib/analysis/Notation.svelte";

	export let data;

	let name = data.name;
	let totalLength = data.source.length;
	let unalteredMelodies, aOrAn, timeOrTimes;

	$: unalteredMelodies = findUnalteredMelodies(data.source);

	$: if (unalteredMelodies) {
		aOrAn = unalteredMelodies.chance % 2 === 0 ? "an" : "a";
		timeOrTimes = unalteredMelodies.melodies.length !== 1 ? "times" : "time";
	}

	function findUnalteredMelodies(arr) {
		const u = [];

		for (const melody of arr) {
			if (Object.keys(melody.truncation).length === 0 && Object.keys(melody.duration).length === 0 && Object.keys(melody.pitches).length === 0) {
				u.push(melody);
			}
		}

		function factorial(n) {
			if (n === 0 || n === 1) {
				return 1;
			}

			return n * factorial(n - 1);
		}

		function binomialCoefficient(y, x) {
			return factorial(y) / (factorial(x) * factorial(y - x));
		}

		function binomialProbability(x, y, p) {
			return binomialCoefficient(y, x) * Math.pow(p, x) * Math.pow(1 - p, y - x);
		}

		const chance = binomialProbability(u.length, arr.length, 0.01) * 100;

		return {
			melodies: u,
			chance: chance.toFixed(1),
		};
	}
</script>

<div class="container">
	<h4>Unaltered melodies</h4>
	{#if unalteredMelodies.melodies.length > 0}
		<p>{data.name} generated <span class="fw-bold">{unalteredMelodies.melodies.length} {unalteredMelodies.melodies.length === 1 ? "unaltered melody" : "unaltered melodies"}</span>!</p>
		{#each unalteredMelodies.melodies as melody, i}
			<h5 class="{i > 0 ? 'mt-3' : ''}">{melody.melodyName}</h5>
			<Notation melody="{melody.originalMelody}" id="{`notation${i}`}" />
		{/each}
	{:else}
		<p>{data.name} didn't generate any unaltered melodies. Remember, it's much more likely for a melody to be unaltered than not.</p>
	{/if}
	<p class="mt-3">
		There's a 1% chance that any given melody will not have a transformation applied to it during the generation process. Because {name} generated {totalLength} melodies during the performance, that means there was approximately {aOrAn}
		{unalteredMelodies.chance}% chance for an unaltered melody to occur {unalteredMelodies.melodies.length}
		{timeOrTimes}.
	</p>
</div>

<style>
	.container {
		max-width: 900px;
	}
</style>
