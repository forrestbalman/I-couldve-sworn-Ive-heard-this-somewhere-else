<script>
	import About from "$lib/analysis/About.svelte";
	import SingleSource from "$lib/analysis/SingleSource.svelte";
</script>

<main id="analysis" class="bg-dark text-light">
	<section id="table-of-contents" class="min-vh-100 container d-flex flex-column justify-content-center align-items-center">
		<h1 class="display-3 text-center mb-3 pt-5">📊 Show me some data! 📈</h1>
		<h5 class="fw-normal text-center lh-base">Aggregation, isolation, and extrapolation of data from MUSC 203G's performance of <br /> <span class="fst-italic">I could've sworn I've heard this somewhere else...</span> 🤔 <br />on October 17th, 2024</h5>
		<div class="container py-5">
			<h2 class="text-center text-decoration-underline mb-3">Table of Contents</h2>
			<nav class="d-flex gap-3 flex-column align-items-center">
				<a class="text-light" href="#about">About</a>
				<a class="text-light fw-light" href="#single-source">Single Source Analysis</a>
				<a class="text-light" href="#multiple-sources">Multi source Analysis</a>
				<a class="text-light" href="#melody-analysis">Performance visualization</a>
			</nav>
		</div>
	</section>
	<About />
	<SingleSource />
</main>
