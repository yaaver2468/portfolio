<script lang="ts">
	import { onMount } from 'svelte';

	let metricValue = $state('');

	onMount(() => {
		const start = performance.now();
		setTimeout(() => {
			const end = performance.now();
			metricValue = `${(end - start).toFixed(2)}ms`;
		}, 0);
	});

	let currentDate = new Date("8/14/2026").toLocaleDateString();
</script>

<footer>
	<div class="social-icons">
		<!-- Placeholders for SVG icons -->
		<div class="icon-placeholder">LinkedIn</div>
		<div class="icon-placeholder">GitHub</div>
	</div>

	<div class="footer-info">
		<p>Website Last Updated: {currentDate}</p>
		<p>Made with Svelte</p>
		{#if metricValue}
			<p>Page loaded in: {metricValue}</p>
		{/if}
	</div>
</footer>

<style>
	footer {
		width: 100%;
		padding: 2rem 0;
		margin-top: 4rem;
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 1.5rem;
		position: relative;
		color: #aaa;
	}

	.social-icons {
		display: flex;
		gap: 2rem;
		font-size: 1.5rem;
	}

	.icon-placeholder {
		cursor: pointer;
		transition: color 0.2s;
	}

	.icon-placeholder:hover {
		color: white;
		text-decoration: underline;
	}

	.footer-info {
		position: absolute;
		right: 0;
		bottom: 0;
		font-size: 0.75rem;
		text-align: right;
		color: #666;
	}

	.footer-info p {
		margin: 0;
	}
</style>
