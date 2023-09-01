<script>
	import { fly } from 'svelte/transition';
	export let xScale;
	export let height;
</script>

{#each xScale.ticks(15) as tick, i}
	<g transform={`translate(${xScale(tick)},${0})`}>
		<line
			x1={0}
			y1={height + 5}
			x2={0}
			y2={height - 5}
			in:fly={{ duration: 500 * i }}
			stroke={'#efefef'}
			stroke-width="1"
		/>

		<text x={0} y={height} dy="20" text-anchor="middle"
			>{tick.toLocaleString('en-US', {
				month: 'numeric',
				day: 'numeric'
				// hour: 'numeric'
				// minute: 'numeric'
			})}</text
		>
	</g>
{/each}

<style>
	line {
		stroke: #c8c8bb;
	}
</style>
