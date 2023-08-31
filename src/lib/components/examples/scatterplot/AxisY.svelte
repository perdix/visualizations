<script>
	export let yScale;
	export let width;
	let yTicks = yScale.ticks(5);
</script>

<g class="axis y">
	{#each yTicks as tick, i}
		<g class="tick" transform="translate(0, {yScale(tick)})">
			<line x1="0" x2={width} y1="0" y2="0" stroke={i == 0 ? 'grey' : '#efefef'} />
			<text x={0} y="0" dy="-5" fill="lightgrey">
				{tick}{i == yTicks.length - 1 ? ' hours studied' : ''}</text
			>
		</g>
	{/each}
</g>

<style>
	.tick text {
		color: black;
		fill: grey;
		font-size: 12px;
	}
</style>
