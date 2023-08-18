<script>
	import Scatterplot from '$lib/components/Scatterplot.svelte';
</script>

<main>
	<h1>Data Visualizations</h1>
	<p>by Paul Opitz</p>
</main>

<section>
	<Scatterplot />
</section>

<style>
	main {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		padding: 100px;
	}
	section {
	}
	h1 {
		font-size: 3rem;
		margin-bottom: 0;
	}
	p {
		font-size: 1.5rem;
		margin-top: 0;
	}
</style>
