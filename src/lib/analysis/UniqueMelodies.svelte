<script>
	export let data;

	const melodyList = ["Stairway to Heaven", "Billie Jean", "What a Wonderful World", "Sweet Child O' Mine", "Hey Jude", "Never Gonna Give You Up", "Livin' on a Prayer", "Dancing Queen", "Don't Stop Believin'", "The Final Countdown", "Y.M.C.A.", "Sweet Home Alabama", "U Can't Touch This", "Careless Whisper"];

	let uniqueMelodies;

	$: uniqueMelodies = findUniqueMelodies(data);

	function findUniqueMelodies(data) {
		let arr = [];

		for (const melody of data.source) {
			if (!arr.includes(melody.melodyName)) {
				arr.push(melody.melodyName);
			}
		}

		return arr;
	}
</script>

<div class="container">
	<h4>Number of Unique Original Melodies</h4>
	<p>Of the 14 original melodies used as source material for this piece, {data.name} used {uniqueMelodies.length} of them</p>
	<div class="d-lg-flex gap-4">
		<div>
			<h5>List of included melodies</h5>
			<ul>
				{#each uniqueMelodies as melody}
					<li>{melody}</li>
				{/each}
			</ul>
		</div>
		<div>
			<h5>List of excluded melodies</h5>
			<ul>
				{#each melodyList as melody}
					{#if !uniqueMelodies.includes(melody)}
						<li>{melody}</li>
					{/if}
				{/each}
			</ul>
		</div>
	</div>
</div>

<style>
	.container {
		max-width: 900px;
	}
</style>
