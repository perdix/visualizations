<script>
	export let yScale;
</script>

{#each yScale.domain() as tick, i}
	<text
		y={yScale(tick) + yScale.bandwidth() / 2}
		text-anchor={'right'}
		dx="-5"
		dominant-baseline="middle">{tick}</text
	>
{/each}

<style>
	text {
		text-anchor: end;
	}
</style>
