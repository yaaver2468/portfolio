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
	<title>Yaaver Imran | Portfolio</title>
	<meta name="description" content="Portfolio of Yaaver Imran, a Fullstack Developer and Systems Consultant specializing in scalable web applications, cloud infrastructure, and custom software solutions." />
	<meta name="keywords" content="Fullstack Developer, SvelteKit, Software Engineer, Systems Consultant, Web Development, Cloud Infrastructure, Portfolio" />
	<meta property="og:title" content="Yaaver Imran | Fullstack Developer & Systems Consultant" />
	<meta property="og:description" content="Explore the projects and professional journey of Yaaver Imran, a specialist in building robust web and system solutions." />
	<meta property="og:type" content="website" />
	<link rel="icon" href={favicon} />
</svelte:head>

<BackgroundCanvas />
<Navbar />

<main>
	{@render children()}
</main>

<Footer />
<ScrollToTop />
