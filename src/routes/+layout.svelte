<script>
	import { fade } from 'svelte/transition';
	import '../app.css';
	import { page } from '$app/stores';

	const charts = [
		{
			name: 'Line Chart',
			path: '/linechart'
		},
		{
			name: 'Bar Chart',
			path: '/barchart'
		},
		{
			name: 'Scatterplot',
			path: '/scatterplot'
		},
		{
			name: 'Beeswarm',
			path: '/beeswarm'
		},
		{
			name: 'Globe',
			path: '/globe'
		},
		{
			name: 'Word Cloud',
			path: '/wordcloud'
		}
	];
</script>

<header>
	<h1>Data Visualizations</h1>
	<p>by Paul Opitz</p>
</header>

<nav>
	<ul>
		{#each charts as chart}
			<li in:fade>
				<a href={chart.path} aria-current={chart.path === $page.route.id}>
					{chart.name}
				</a>
			</li>
		{/each}
	</ul>
</nav>

<main>
	<slot />
</main>

<style>
	header {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		padding: 60px;
	}
	h1 {
		font-size: 3rem;
		margin-bottom: 0;
	}

	p {
		font-size: 1.5rem;
		margin-top: 0;
	}

	nav {
		display: flex;
		justify-content: center;
		padding: 30px;
	}
	nav ul {
		display: flex;
		list-style: none;
		padding: 0;
		margin: 0;
	}
	nav li {
		margin: 0 15px;
	}
	nav a {
		text-decoration: none;
		color: #999;
		font-size: 1.5rem;
	}
	nav a:hover,
	nav a[aria-current='true'] {
		color: #000;
	}
	main {
		padding: 60px;
	}
</style>
