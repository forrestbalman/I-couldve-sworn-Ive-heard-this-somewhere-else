<script>
	export let data;

	let mostCommonMelody;

	$: mostCommonMelody = findMostCommonMelody(data.source);

	function findMostCommonMelody(arr) {
		let melodyName = "";
		let largestCount = 0;
		let counter = 0;
		let mostCommonMelodies = [];

		for (let i = 0; i < arr.length; i++) {
			melodyName = arr[i].melodyName;
			counter = 0;

			for (let j = 0; j < arr.length; j++) {
				if (melodyName === arr[j].melodyName) {
					counter++;
				}
			}

			if (counter >= largestCount) {
				largestCount = counter;
				if (!mostCommonMelodies.includes(melodyName)) {
					mostCommonMelodies.push(melodyName);
				}
			}
		}

		return {
			titles: mostCommonMelodies,
			count: largestCount,
		};
	}
</script>

<div class="container">
	<h4>Most common {mostCommonMelody.titles.length > 1 ? "melodies" : "melody"}</h4>
	<p>
		{#if mostCommonMelody.titles.length > 1}
			There were {mostCommonMelody.titles.length} melodies that were the most common source material, each appearing <span class="fw-bold">{mostCommonMelody.count}</span> times:
			<br />
			{#each mostCommonMelody.titles as title, i}
				{#if i === mostCommonMelody.titles.length - 1}
					and <span class="fw-bold">{title}</span>.
				{:else}
					<span class="fw-bold">{title}</span>{", "}
				{/if}
			{/each}
		{:else}
			There was {mostCommonMelody.titles.length} melody that was the most common source material, appearing <span class="fw-bold">{mostCommonMelody.count}</span> times: <span class="fw-bold">{mostCommonMelody.titles[0]}</span>.
		{/if}
	</p>
</div>

<style>
	.container {
		max-width: 900px;
	}
</style>
