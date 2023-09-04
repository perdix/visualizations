<script>
	import Scatterplot from '$lib/components/examples/scatterplot/Scatterplot.svelte';
	import Beeswarmplot from '$lib/components/examples/beeswarmplot/Beeswarmplot.svelte';
	import Globe from '$lib/components/examples/globe/Globe.svelte';
</script>

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
	section {
		margin: 15px;
	}
</style>
