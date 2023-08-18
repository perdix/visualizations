<script>
	let xTicks = [0, 20, 40, 60, 80, 100];
	export let height;
	export let width;
	export let xScale;
</script>

<g class="axis x" transform="translate(0 {height})">
	{#each xTicks as tick, i}
		<g class="tick" transform="translate({xScale(tick)} 0)">
			<line x1="0" x2="0" y1="0" y2="6" stroke="grey" />
			<text x={0} y={6} dy={9} dominant-baseline="middle" text-anchor={'middle'}> {tick}%</text>
		</g>
	{/each}
	<text class="axis-title" x={width} y="0" text-anchor="end" dominant-baseline="start" dy="-6"
		>Final Grade
	</text>
</g>
