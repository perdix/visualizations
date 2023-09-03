<script>
	export let yScale;
	export let width;
	let yTicks = yScale.ticks(10);
</script>

{#each yTicks as tick, i}
	<g class="tick" transform="translate(0, {yScale(tick)})">
		<line x1="0" x2={width} y1="0" y2="0" stroke={i == 0 ? 'grey' : '#efefef'} />
		<text x={0} y="0" dx="-30" fill="lightgrey" dominant-baseline="middle" text-anchor="center">
			{tick}°</text
		>
	</g>
{/each}

<style>
	text {
		fill: black;
	}
</style>
