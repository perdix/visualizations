<script>
	export let colorScale;
	export let hoveredContinent;
</script>

<!-- svelte-ignore a11y-no-static-element-interactions -->
<div class="legend" on:mouseleave={() => (hoveredContinent = null)}>
	{#each colorScale.domain() as continent, i}
		<!-- svelte-ignore a11y-mouse-events-have-key-events -->
		<!-- svelte-ignore a11y-no-static-element-interactions -->
		<div
			class="legend-item"
			class:unhovered={hoveredContinent && hoveredContinent !== continent}
			transform="translate(0 {i * 20})"
			on:mouseover={() => (hoveredContinent = continent)}
		>
			<div class="circle" style="background-color:{colorScale(continent)}" />
			<div class="text">
				{continent}
			</div>
		</div>
	{/each}
</div>

<style>
	.legend {
		display: flex;
		justify-content: center;
		align-items: center;
		gap: 10px;
		padding: 5px;
	}
	.unhovered {
		opacity: 0.5;
	}
	.legend-item {
		display: flex;
		justify-content: flex-start;
		align-items: center;
		text-transform: uppercase;
		transition: opacity 200ms ease;
		cursor: pointer;
	}
	.circle {
		width: 15px;
		height: 15px;
		margin-right: 5px;
		border-radius: 50%;
		display: inline-block;
		margin-right: 3px;
		border: 1px solid black;
	}
</style>
