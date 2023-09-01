<script>
	import dataset from './dataset.json';
	import { scaleBand, scaleLinear } from 'd3-scale';
	import { max } from 'd3-array';
	import { slide, fade } from 'svelte/transition';
	import AxisX from './AxisX.svelte';
	import AxisY from './AxisY.svelte';

	let width;
	let height = 400;

	const margin = {
		top: 20,
		right: 30,
		bottom: 50,
		left: 80
	};

	const innerHeight = height - margin.top - margin.bottom;
	$: innerWidth = width - margin.left - margin.right;

	const yScale = scaleBand()
		.domain(dataset.map((d) => d['fruit']))
		.range([0, innerHeight])
		.paddingInner(0.15);

	$: xScale = scaleLinear()
		.domain([0, max(dataset, (d) => d.quantity)])
		.range([0, innerWidth]);
</script>

<h1>Barchart</h1>

<div class="container" bind:clientWidth={width}>
	{#if width > 0}
		<svg {width} {height} in:fade|global={{ duration: 100 }}>
			<g transform="translate({margin.left} {margin.top})">
				<AxisX {xScale} height={innerHeight} />
				<AxisY {yScale} />
				{#each dataset as data, i}
					<rect
						in:slide|global={{ duration: 300, axis: 'x', delay: i * 50 }}
						x={0}
						y={yScale(data.fruit)}
						width={xScale(data.quantity)}
						height={yScale.bandwidth()}
						fill="hsl({Math.floor(Math.random() * 360)}, 50%, 80%)"
					/>
				{/each}
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
</style>
