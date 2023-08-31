<script>
	import data from './data.js';
	import { scaleLinear } from 'd3-scale';
	import AxisX from './AxisX.svelte';
	import AxisY from './AxisY.svelte';
	import { fly } from 'svelte/transition';

	let width = 0;
	let height = 600;

	let tooltipWidth = 0;
	let tooltipHeight = 0;

	let margin = {
		top: 30,
		right: 30,
		bottom: 30,
		left: 30
	};

	$: innerWidth = width - margin.left - margin.right;
	let innerHeight = height - margin.top - margin.bottom;

	$: xScale = scaleLinear().domain([0, 100]).range([0, innerWidth]);
	let yScale = scaleLinear().domain([0, 60]).range([innerHeight, 0]);

	/** @type { { name: string, grade: number, hours: number } | null } */
	let hoveredData;
	$: x = xScale(hoveredData?.grade) + margin.left;
	$: xPos = tooltipWidth + x > width ? x - tooltipWidth : x;

	$: y = yScale(hoveredData?.hours) + margin.top;
</script>

<!-- svelte-ignore a11y-no-static-element-interactions -->
<div
	class="container"
	bind:clientWidth={width}
	on:mouseleave={() => {
		hoveredData = null;
	}}
>
	{#if width}
		<h1>Students and Grades</h1>
		<svg {width} {height}>
			<g transform="translate({margin.left} {margin.top})">
				<AxisX {xScale} height={innerHeight} width={innerWidth} />
				<AxisY {yScale} width={innerWidth} />
				{#each data.sort((a, b) => a.grade - b.grade) as d}
					<!-- svelte-ignore a11y-no-static-element-interactions -->
					<!-- svelte-ignore a11y-no-noninteractive-tabindex -->
					<circle
						in:fly|global={{ y: 15, duration: 1000 }}
						cx={xScale(d.grade)}
						cy={yScale(d.hours)}
						r={hoveredData === d ? 18 : 10}
						fill={hoveredData === d || !hoveredData
							? 'rgba(255, 173, 71, 1)'
							: 'rgba(255, 173, 71, 0.5)'}
						stroke-width="1"
						on:focus={() => {
							hoveredData = d;
						}}
						on:mouseover={() => {
							hoveredData = d;
						}}
						tabindex="0"
					/>
				{/each}
			</g>
		</svg>
		{#if hoveredData}
			<div
				in:fly={{ y: -20, duration: 300 }}
				out:fly={{ y: 20, duration: 300 }}
				bind:clientWidth={tooltipWidth}
				bind:clientHeight={tooltipHeight}
				class="tooltip"
				style="top: {y}px; left: {xPos}px"
			>
				{#if hoveredData.name}
					<h2>{hoveredData.name}</h2>
				{/if}
				<p>Grade: {hoveredData.grade}%</p>
				<p>Studied {hoveredData.hours} Hours</p>
			</div>
		{/if}
	{/if}
</div>

<style>
	:global(.tick text, .axis-title) {
		font-weight: 400;
		font-size: 12px;
		fill: grey;
	}
	.container {
		position: relative;
	}
	.tooltip {
		position: absolute;
		background-color: white;
		padding: 10px;
		border-radius: 5px;
		transition: all 0.3s ease-in-out;
		box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
		pointer-events: none;
	}
	.tooltip h2 {
		font-size: 1rem;
	}
	circle {
		transition: all 0.2s ease-in-out;
	}
	circle:hover {
		cursor: pointer;
	}
	h1 {
		text-align: center;
		font-weight: 100;
		font-size: 30px;
	}
</style>
