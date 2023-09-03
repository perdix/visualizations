<script>
	import { scaleTime, scaleLinear } from 'd3-scale';
	import { line, curveNatural } from 'd3-shape';
	import { extent, bisector } from 'd3-array';
	import { slide, fade } from 'svelte/transition';
	import AxisX from './AxisX.svelte';
	import AxisY from './AxisY.svelte';
	import dataset from './dataset.json';

	let width;
	let height = 400;

	const margin = {
		top: 20,
		right: 30,
		bottom: 50,
		left: 80
	};

	const tempData = dataset.map((d) => ({
		temperature: d.temperature,
		timestamp: new Date(d.timestamp)
	}));

	const innerHeight = height - margin.top - margin.bottom;
	$: innerWidth = width - margin.left - margin.right;

	$: xScale = scaleTime()
		.domain(extent(tempData, (d) => d.timestamp))
		.range([0, innerWidth]);

	const yScale = scaleLinear()
		.domain(extent(tempData, (d) => d.temperature))
		.range([innerHeight, 0])
		.nice();

	$: generatedLine = line()
		.curve(curveNatural)
		.x((d) => xScale(d.timestamp))
		.y((d) => yScale(d.temperature))(tempData);

	let circleX;
	let circleY;

	let bisect = bisector((d) => d.timestamp).center;
	let sortedTempData = [...tempData].sort((a, b) => a.timestamp - b.timestamp);

	const setMousePos = (e) => {
		if (e.clientX < margin.left || e.clientX > width - margin.right) return;
		// Get the x value of the mouse position
		circleX = e.clientX - margin.left;
		// Get the original x value of the dataset
		const originalX = xScale.invert(circleX);
		// Calculate the original y value of the dataset
		const index = bisect(sortedTempData, originalX);
		// Set the y value
		circleY = yScale(sortedTempData[index].temperature);
	};
	const unsetMousePos = () => {
		circleX = null;
		circleY = null;
	};
</script>

<h1>Line Chart</h1>

<!-- svelte-ignore a11y-mouse-events-have-key-events -->
<!-- svelte-ignore a11y-no-static-element-interactions -->
<div class="container" bind:clientWidth={width}>
	{#if width > 0}
		<svg
			{width}
			{height}
			in:fade|global={{ duration: 100 }}
			on:mousemove={setMousePos}
			on:mouseleave={unsetMousePos}
		>
			<g transform="translate({margin.left} {margin.top})">
				<AxisX {xScale} height={innerHeight} />
				<AxisY {yScale} width={innerWidth} />
				<path d={generatedLine} />

				{#each tempData as data, i}
					<!-- <circle
						cx={xScale(data.timestamp)}
						cy={yScale(data.temperature)}
						r="5"
						fill="firebrick"
					/> -->
				{/each}
				{#if circleX && circleY}
					<circle cx={circleX} cy={circleY} r="10" />
				{/if}
			</g>
		</svg>
	{/if}
</div>

<style>
	.container {
		margin: 0 auto;
	}
	h1 {
		margin-top: 30px;
		text-align: center;
		font-weight: 100;
		font-size: 30px;
	}
	path {
		fill: transparent;
		stroke: rgb(18, 153, 90);
		stroke-width: 3;
		stroke-linejoin: round;
	}
	circle {
		fill: rgb(18, 153, 90);
	}
</style>
