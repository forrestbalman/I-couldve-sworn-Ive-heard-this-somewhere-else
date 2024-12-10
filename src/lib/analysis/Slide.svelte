<script>
	export let id, title, img;
	let path = "/I-couldve-sworn-Ive-heard-this-somewhere-else";
</script>

<div id="slide-{id}" class="h-100 d-flex flex-column">
	<h1 class="text-center">{title}</h1>
	<div class="flex-grow-1">
		<img src="{path}/slides/{img}" alt="{title}" class="img-fluid h-100" />
	</div>
</div>

<style>
	div {
		width: 100%;
		height: 100%;
		position: relative;
	}
</style>
