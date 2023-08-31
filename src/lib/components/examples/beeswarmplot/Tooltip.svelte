<script>
	import Tooltip from './Tooltip.svelte';
	export let data;
	export let margin;
	export let colorScale;
	export let width;
	import { fly, fade } from 'svelte/transition';
	let toolTipWidth = 0;

	let xPos = 0;
	let yPos = 0;
	let xNudge = 100;

	$: xPos =
		data.x + toolTipWidth + xNudge < width
			? data.x + margin.left
			: data.x - toolTipWidth + margin.left;
	$: yPos = data.y + margin.top;
</script>

<div
	class="tooltip"
	style="top:{yPos}px; left:{xPos}px;"
	in:fly={{ y: 10, duration: 200, delay: 200 }}
	out:fade
	bind:clientWidth={toolTipWidth}
>
	<div class="tooltip-title">
		{data.country}
	</div>
	<div class="tooltip-text">
		Happiness: {data.happiness}
	</div>
	<div class="continent" style="background: {colorScale(data.continent)}">
		{data.continent}
	</div>

	<span class="bar background" />
	<span
		class="bar foreground"
		style="background: {colorScale(data.continent)}; width: {data.happiness * 10}%"
	/>
</div>

<style>
	.tooltip {
		position: absolute;
		background-color: white;
		padding: 15px;
		border-radius: 5px;
		box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
		pointer-events: none;
		transition: top 300ms ease, left 300ms ease;
	}
	.tooltip-title {
		font-weight: bold;
		font-size: 1.1rem;
	}
	.tooltip-text {
		font-size: 1rem;
	}
	.continent {
		font-size: 0.8rem;
		padding: 5px;
		border-radius: 5px;
		text-transform: uppercase;
		display: inline-block;
		margin-top: 5px;
	}
	.bar {
		position: absolute;
		height: 3px;
		width: 100%;
		bottom: 0;
		left: 0;
	}
	.bar.background {
		background: #ddd;
	}
</style>
