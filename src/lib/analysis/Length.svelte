<script>
	export let data;

	let length;

	$: length = data.source.length;
</script>

<div class="container">
	<h4>Length</h4>
	<p>{data.name} generated a total of <span class="fw-bold">{length}</span> melodies.</p>
</div>

<style>
	.container {
		max-width: 900px;
	}
</style>
