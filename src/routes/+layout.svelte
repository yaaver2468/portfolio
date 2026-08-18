<script lang="ts">
	import "../app.css";
	import favicon from "$lib/assets/favicon.ico";
	import Navbar from "$lib/components/Navbar.svelte";
	import BackgroundCanvas from "$lib/components/BackgroundCanvas.svelte";
	import Footer from "$lib/components/Footer.svelte";
	import ScrollToTop from "$lib/components/ScrollToTop.svelte";

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

<BackgroundCanvas />
<Navbar />

<main>
	{@render children()}
</main>

<Footer />
<ScrollToTop />
