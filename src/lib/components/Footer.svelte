<script lang="ts">
	import { onMount } from "svelte";
	import github from "$lib/assets/GitHub_Invertocat_White_Clearspace.svg?url";
	import linkedin from "$lib/assets/InBug-White.png";

	let metricValue = $state("");

	onMount(() => {
		const start = performance.now();
		setTimeout(() => {
			const end = performance.now();
			metricValue = `${(end - start).toFixed(2)}ms`;
		}, 0);
	});
</script>

<footer>
	<div class="social-icons">
		<a href="https://github.com/yaaver2468" title="GitHub" target="_blank">
			<img src={github} alt="Github Icon" />
		</a>

		<a href="https://linkedin.com/in/yaaverimran" title="LinkedIn" target="_blank">
			<img style="padding: 5px;" src={linkedin} alt="LinkedIn Icon" />
		</a>
	</div>

	<div class="footer-info">
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
	}

	.social-icons {
		display: flex;
		gap: 2rem;
		font-size: 1.5rem;
	}

	.social-icons {
		display: flex;
		gap: 2rem;
	}

	.social-icons img {
		height: 48px;
		display: block;
	}

	.footer-info {
		position: absolute;
		right: 0;
		bottom: 36px;
		font-size: 0.75rem;
		text-align: right;
		color: color-mix(in srgb, var(--global-white) 70%, transparent);
	}

	.footer-info p {
		margin: 0;
	}
</style>
