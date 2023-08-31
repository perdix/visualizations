<script>
	export let colorScale;
	export let data;
	import { format } from 'd3-format';
	import { fade } from 'svelte/transition';
	const suffixFormat = (d) => format('.2s')(d).replace('G', 'B');

	let maxValue = colorScale.domain()[1];
	$: percentOfMax = (data?.population / maxValue) * 100;
</script>

<div class="legend">
	<span class="label">{colorScale.domain()[0]}</span>

	<div
		class="bar"
		style="background: linear-gradient(to right, {colorScale.range()[0]} 0%,  {colorScale.range()[1]} 100%);"
	>
		{#if percentOfMax}
			<span class="line" style="left:{percentOfMax}%" transition:fade />
		{/if}
	</div>

	<span class="label">{suffixFormat(maxValue)}</span>
</div>

<style>
	.legend {
		margin-top: 30px;
		display: flex;
		justify-items: space-between;
		gap: 6px;
	}
	.bar {
		height: 15px;
		width: 100%;
		position: relative;
	}
	.label {
		color: white;
		font-size: 0.8rem;
		user-select: none;
	}
	.line {
		position: absolute;
		top: 0;
		height: 15px;
		background: white;
		width: 2px;
		transition: left 0.4s ease-out;
	}
</style>
