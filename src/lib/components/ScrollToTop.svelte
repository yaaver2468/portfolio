<script lang="ts">
	import { onMount } from "svelte";
	import Arrow from "$lib/assets/arrow.svg?component";

	let isVisible = $state(false);

	onMount(() => {
		const handleScroll = () => {
			isVisible = window.scrollY > 50;
		};

		window.addEventListener("scroll", handleScroll, { passive: true });
		return () => window.removeEventListener("scroll", handleScroll);
	});

	function scrollToTop() {
		window.scrollTo({
			top: 0,
		});
	}
</script>

{#if isVisible}
	<button class="scroll-to-top" onclick={scrollToTop} aria-label="Scroll to top">
		<Arrow />
	</button>
{/if}

<style>
	.scroll-to-top :global {
		position: fixed;
		bottom: 2rem;
		right: 2rem;
		width: 3rem;
		height: 3rem;
		border-radius: 50%;
		background-color: color-mix(in srgb, var(--global-bg) 70%, black);
		border: none;
		display: flex;
		align-items: center;
		justify-content: center;
		cursor: pointer;
		box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
		transition: transform 0.2s ease, opacity 0.3s ease;
		z-index: 1000;
		svg {
			width: 1.5rem;
			height: 1.5rem;
			color: var(--global-white);
			transform: rotateX(180deg);
		}
	}

	.scroll-to-top:hover {
		transform: scale(1.1);
		background-color: color-mix(in srgb, var(--global-bg) 80%, transparent);
	}

	.scroll-to-top:active {
		transform: scale(0.95);
	}

	@media (max-width: 768px) {
		.scroll-to-top {
			bottom: 1rem;
			right: 1rem;
			width: 2.5rem;
			height: 2.5rem;
		}
	}
</style>
