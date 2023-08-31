<script>
	export let data;
	import { format } from 'd3-format';
	import { fade } from 'svelte/transition';
	const suffixFormat = (d) => format('.2s')(d).replace('G', 'B');
</script>

{#if data?.country}
	<div transition:fade>
		<h2>{data.country}</h2>
		<h3>Population of {suffixFormat(data.population)}</h3>
	</div>
{/if}

<style>
	div {
		position: absolute;
		bottom: 30px;
		right: 0px;
		text-align: right;
	}
	h2 {
		font-size: 1.5rem;
		color: white;
		font-weight: 200;
	}
	h3 {
		font-size: 1.1rem;
		color: rgba(255, 255, 255, 0.8);
		font-weight: 100;
	}
</style>
