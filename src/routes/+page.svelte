<script>
	import Scatterplot from '$lib/components/examples/scatterplot/Scatterplot.svelte';
	import Beeswarmplot from '$lib/components/examples/beeswarmplot/Beeswarmplot.svelte';
	import Globe from '$lib/components/examples/globe/Globe.svelte';
</script>

<main>
	<h1>Data Visualizations</h1>
	<p>by Paul Opitz</p>
</main>

<section>
	<h2>Scatterplot</h2>
	<Scatterplot />
</section>

<section>
	<h2>Beeswarm</h2>
	<Beeswarmplot />
</section>

<section>
	<h2>Globe</h2>
	<Globe />
</section>

<style>
	main {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		padding: 60px;
	}
	section {
		margin: 15px;
	}
	h1 {
		font-size: 3rem;
		margin-bottom: 0;
	}
	h2 {
		text-align: center;
		margin-top: 60px;
	}
	p {
		font-size: 1.5rem;
		margin-top: 0;
	}
</style>
