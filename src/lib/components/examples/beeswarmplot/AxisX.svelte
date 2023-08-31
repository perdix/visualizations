<script>
	export let height;
	export let width;
	export let xScale;

	$: ticks = xScale.ticks(4);
</script>

<g class="axis-x">
	{#each ticks as tick}
		<g class="tick" transform="translate({xScale(tick)}, 0)">
			<line y2={height} stroke="lightgrey" />
			<text y={height + 15} fill="lightgrey" text-anchor="middle">
				{tick}
			</text>
		</g>
	{/each}
	<text
		class="axis-title"
		x={width / 2}
		y={height}
		text-anchor="middle"
		dominant-baseline="hanging"
		dy="20"
	>
		&larr; Happiness &rarr;</text
	>
</g>

<style>
	.tick text {
		color: black;
		fill: grey;
		font-size: 12px;
	}
</style>
