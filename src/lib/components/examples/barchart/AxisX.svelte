<script>
	import { fly } from 'svelte/transition';
	export let xScale;
	export let height;
</script>

{#each xScale.ticks() as tick, i}
	<g transform={`translate(${xScale(tick)},${0})`}>
		<line x1="0" y1={0} x2={0} y2={height} in:fly={{ duration: 500 * i }} />

		<text x={0} y={height} dy="20" text-anchor="middle">{tick}</text>
	</g>
{/each}

<style>
	line {
		stroke: #c8c8bb;
	}
</style>
