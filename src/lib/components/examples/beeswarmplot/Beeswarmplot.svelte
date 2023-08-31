<script>
	import { forceSimulation, forceY, forceX, forceCollide } from 'd3-force';
	import data from './data.js';
	import { scaleBand, scaleLinear, scaleOrdinal, scaleSqrt } from 'd3-scale';
	import { mean, rollups } from 'd3-array';
	import AxisX from './AxisX.svelte';
	import AxisY from './AxisY.svelte';
	import Legend from './Legend.svelte';
	import Tooltip from './Tooltip.svelte';
	import { fly, fade } from 'svelte/transition';
	const RADIUS = 5;
	let hoveredContinent;
	let groupByContinent = false;

	let simulation = forceSimulation(data);

	$: {
		simulation
			.force('x', forceX((d) => xScale(d.happiness)).strength(0.8))
			.force(
				'y',
				forceY((d) => (groupByContinent ? yScale(d.continent) : innerHeight / 2)).strength(0.8)
			)
			.force(
				'collide',
				forceCollide().radius((d) => radiusScale(d.happiness) + 0.15)
			)
			.alpha(0.3)
			.alphaDecay(0.01)
			.restart();
	}

	let nodes = [];
	simulation.on('tick', () => {
		nodes = simulation.nodes();
	});

	let width = 400;
	let height = 600;

	const margin = {
		top: 20,
		right: 0,
		bottom: 40,
		left: 20
	};

	const continents = rollups(
		data,
		(v) => mean(v, (d) => d.happiness),
		(d) => d.continent
	) // Group data by continent and return the group-wide mean
		.sort((a, b) => a[1] - b[1]) // Sort according to value
		.map((d) => d[0]); // Grab the continent name

	$: innerWidth = width - margin.left - margin.right;
	$: innerHeight = height - margin.top - margin.bottom;

	$: xScale = scaleLinear().domain([1, 9]).range([0, innerWidth]);
	$: yScale = scaleBand().domain(continents).range([innerHeight, 0]).paddingInner(0.2);

	const colorRange = ['#dda0dd', '#fe7f2d', '#fcca46', '#a1c181', '#619b8a', '#eae2b7'];
	let colorScale = scaleOrdinal()
		.domain(continents) // continents was already defined in our code
		.range(colorRange);
	let radiusScale = scaleSqrt().domain([1, 9]).range([3, 8]);

	let hovered;
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-static-element-interactions -->
<div
	class="container"
	bind:clientWidth={width}
	on:click={() => {
		groupByContinent = !groupByContinent;
		hovered = null;
	}}
>
	<h1>The happiest countries in the world</h1>
	<Legend {colorScale} bind:hoveredContinent />

	<!-- svelte-ignore a11y-no-static-element-interactions -->
	<svg
		{width}
		{height}
		on:mouseleave={() => {
			hovered = null;
		}}
	>
		<g class="inner-chart" transform="translate({margin.left}, {margin.top})">
			<AxisX width={innerWidth} height={innerHeight} {xScale} />
			<AxisY {yScale} {groupByContinent} />

			{#if hovered}
				<line
					transition:fade={{ duration: 200 }}
					x1={hovered.x}
					x2={hovered.x}
					y1={hovered.y}
					y2={innerHeight}
					stroke={colorScale(hovered.continent)}
					stroke-width="2"
					opacity="0.9"
				/>
			{/if}

			{#each nodes as node, i}
				<!-- svelte-ignore a11y-no-noninteractive-tabindex -->
				<circle
					in:fade={{ delay: 400 + i * 10 }}
					cx={node.x}
					cy={node.y}
					r={radiusScale(node.happiness)}
					fill={colorScale(node.continent)}
					stroke={hovered || hoveredContinent
						? hovered === node || hoveredContinent === node.continent
							? 'black'
							: 'transparent'
						: '#00000090'}
					opacity={hovered || hoveredContinent
						? hovered === node || hoveredContinent === node.continent
							? '1'
							: '0.3'
						: '1'}
					stroke-width="1"
					on:mouseover={() => {
						hovered = node;
					}}
					on:focus={() => {
						hovered = node;
					}}
					tabindex="0"
					on:click={(e) => {
						e.stopPropagation();
					}}
				/>
			{/each}
		</g>
	</svg>
	{#if hovered}
		<Tooltip data={hovered} {margin} {colorScale} {width} />
	{/if}
</div>

<style>
	h1 {
		text-align: center;
		font-weight: 100;
		font-size: 30px;
	}
	circle {
		transition: stroke 300ms ease, opacity 300ms ease;
		cursor: pointer;
	}
</style>
