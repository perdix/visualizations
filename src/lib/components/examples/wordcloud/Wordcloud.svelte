<script>
	import data from './data.json';
	import d3Cloud from 'd3-cloud';
	import { descending, extent, rollups } from 'd3-array';
	import { onMount } from 'svelte';
	import { browser } from '$app/environment';

	// Should be done with a real tokenizer
	const allLyrics = data.reduce((acc, i) => acc + ' ' + i.text, '');
	const tokens = allLyrics.trim().split(/\s+/g);

	const preparedData = rollups(
		tokens,
		(g) => g.length,
		(w) => w.toLowerCase()
	)
		.sort(([, a], [, b]) => descending(a, b))
		.map(([key, size]) => ({ text: key, size }));

	const margin = {
		top: 15,
		right: 15,
		bottom: 15,
		left: 15
	};
	let width;
	let height = 600;

	$: innerWidth = width - margin.left - margin.right;
	const innerHeight = height - margin.top - margin.bottom;

	const wordPadding = 2;
	$: cloudWords = [];

	$: if (browser && width) {
		const cloud = d3Cloud()
			.size([innerWidth, innerHeight])
			.words(preparedData)
			.padding(wordPadding)
			.rotate(0)
			.fontSize((d) => Math.sqrt(d.size) * 20)
			.on('word', ({ size, x, y, rotate, text }) => {
				cloudWords.push({ size, x, y, rotate, text });
			});
		cloud.start();
		// words = [...cloudWords];
	}

	onMount(() => {});
</script>

<div bind:clientWidth={width}>
	{#if width}
		<svg width={innerWidth} height={innerHeight} text-anchor="middle">
			<g transform={`translate(${margin.left} ${margin.top})`}>
				{#each cloudWords as word}
					<text
						font-size={word.size}
						transform={`translate(${word.x}, ${word.y}) rotate(${word.rotate})`}
						fill="#CC2936">{word.text}</text
					>
				{/each}
			</g>
		</svg>
	{/if}
</div>
