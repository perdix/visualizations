<script>
	import { scaleLinear } from 'd3-scale';
	import world from './world-110m.json';
	import * as topojson from 'topojson-client';
	import data from './data.json';
	import { max } from 'd3-array';
	import { timer } from 'd3-timer';
	import { drag } from 'd3-drag';
	import { onMount } from 'svelte';
	import { select } from 'd3-selection';
	import { spring } from 'svelte/motion';
	import { geoPath, geoOrthographic, geoCentroid } from 'd3-geo';
	import Tooltip from './Tooltip.svelte';
	import { draw } from 'svelte/transition';
	import Legend from './Legend.svelte';

	let countries = topojson.feature(world, world.objects.countries).features;
	let borders = topojson.mesh(world, world.objects.countries, (a, b) => a !== b);

	// Add data to countries
	countries.forEach((c) => {
		let metadata = data.find((d) => d.id === c.id);
		if (metadata) {
			c.population = metadata.population;
			c.country = metadata.country;
		}
	});

	let width = 400;
	$: height = width;

	$: projection = geoOrthographic()
		.scale(width / 2)
		.rotate([$xRotation, $yRotation, 0])
		.translate([width / 2, height / 2]);

	$: path = geoPath(projection);

	let colorScale = scaleLinear()
		.domain([0, max(data, (d) => d.population)])
		.range(['#26362e', '#0dcc6c']);

	let xRotation = spring(0, { stiffness: 0.08, damping: 0.4 });
	let yRotation = spring(0, { stiffness: 0.17, damping: 0.7 });

	let degreesPerFrame = 0.2;

	const t = timer((elapsed) => {
		if (dragging || selectedCountry) return;
		$xRotation += degreesPerFrame;
	}, 0);

	let globe;
	let dragging = false;
	const DRAG_SENSITIVITY = 0.5;

	let selectedCountry;

	$: if (selectedCountry) {
		const center = geoCentroid(selectedCountry);
		$xRotation = -center[0];
		$yRotation = -center[1];
	}

	onMount(() => {
		const element = select(globe);
		element.call(
			drag()
				.on('drag', (event) => {
					dragging = true;
					$xRotation = $xRotation + event.dx * DRAG_SENSITIVITY;
					$yRotation = $yRotation - event.dy * DRAG_SENSITIVITY;
				})
				.on('end', (event) => {
					dragging = false;
				})
		);
	});
</script>

<h1>Globe</h1>

<div class="bg">
	<div class="container" bind:clientWidth={width}>
		<svg {width} {height} bind:this={globe} class:dragging>
			<defs>
				<filter id="glow" height="120%">
					<feGaussianBlur in="SourceAlpha" stdDeviation="15" />
					<feOffset dx="0" dy="0" result="offsetblur" />
					<feFlood flood-color="rgba(100, 255, 100, .55)" />
					<feComposite in2="offsetblur" operator="in" />
					<feMerge>
						<feMergeNode />
						<feMergeNode in="SourceGraphic" />
					</feMerge>
				</filter>
			</defs>

			<!-- svelte-ignore a11y-click-events-have-key-events -->
			<!-- svelte-ignore a11y-no-static-element-interactions -->
			<!-- svelte-ignore a11y-no-noninteractive-tabindex -->
			<circle
				cx={width / 2}
				cy={height / 2}
				r={width / 2}
				fill="#1c1c1d"
				filter="url(#glow)"
				on:click={() => (selectedCountry = null)}
				on:focus={() => (selectedCountry = null)}
				tabindex="0"
			/>

			{#each countries as country}
				<!-- svelte-ignore a11y-no-noninteractive-tabindex -->
				<!-- svelte-ignore a11y-click-events-have-key-events -->
				<!-- svelte-ignore a11y-no-static-element-interactions -->
				<path
					d={path(country)}
					fill={colorScale(country?.population || 0)}
					stroke="none"
					on:click={() => (selectedCountry = country)}
					on:focus={() => (selectedCountry = country)}
					tabindex="0"
				/>
			{/each}
			<path d={path(borders)} fill="none" stroke="black" />

			{#if selectedCountry}
				{#key selectedCountry.id}
					<path
						d={path(selectedCountry)}
						fill="transparent"
						stroke="white"
						stroke-width="2"
						pointer-events="none"
						in:draw
					/>
				{/key}
			{/if}
		</svg>
		<Tooltip data={selectedCountry} />
		<Legend {colorScale} data={selectedCountry} />
	</div>
</div>

<style>
	.bg {
		padding: 30px;
		background-color: rgba(50, 50, 50, 1);
	}
	.container {
		max-width: 600px;
		margin: 0 auto;
	}
	h1 {
		text-align: center;
		font-weight: 100;
		font-size: 30px;
	}
	svg {
		overflow: visible;
	}
	.dragging {
		cursor: grabbing;
	}

	path {
		cursor: pointer;
	}
	path:focus,
	circle:focus {
		outline: none;
	}
</style>
