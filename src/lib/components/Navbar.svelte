<script lang="ts">
	import Logo from "$lib/assets/logo.svg?component";

	const links = [
		{ href: "/", label: "Home" },
		{ href: "/about", label: "About" },
		{ href: "/contact", label: "Contact" },
	];

	let isScrolled = $state(false);

	function handleScroll() {
		isScrolled = window.scrollY > 50;
	}
</script>

<svelte:window onscroll={handleScroll} />

<div class="nav-wrapper">
	<nav class:scrolled={isScrolled}>
		<div class="nav-container">
			<div class="logo">
				<a href="/" title="Home">
					<Logo />
				</a>
			</div>
			<ul class="nav-links">
				{#each links as link}
					<li>
						<a href={link.href}>{link.label}</a>
					</li>
				{/each}
			</ul>
		</div>
	</nav>
</div>

<style>
	.nav-wrapper {
		--mobile-nav-height: 72px;
		--nav-height: 88px;
		height: var(--nav-height);
		width: 100%;
	}

	nav {
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: var(--nav-height);
		box-sizing: border-box;
		padding: 1rem 0;
		background-color: transparent;
		z-index: 100;
		transition:
			background-color 0.3s ease,
			backdrop-filter 0.3s ease,
			box-shadow 0.3s ease,
			padding 0.3s ease;
	}

	nav.scrolled {
		background-color: color-mix(in srgb, var(--global-bg) 80%, transparent);;
		backdrop-filter: blur(10px);
		padding: 0.5rem 0;
		box-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
	}

	.nav-container {
		width: 90%;
		height: 100%;
		margin: 0 auto;
		display: flex;
		justify-content: space-between;
		align-items: center;
	}

	.logo :global {
		svg {
			color: var(--global-white);
			max-width: 100px;
			max-height: 100px;
			display: block;
		}
	}

	.nav-links {
		display: flex;
		list-style: none;
		margin: 0;
		padding: 0;
		gap: 2rem;
	}

	.nav-links a {
		color: var(--global-white);
		text-decoration: none;
		font-weight: bold;
		font-size: 1.5rem;
		transition: color 0.2s ease;
	}

	.nav-links a:hover {
		color: white;
	}

	@media (min-width: 768px) {
		.nav-links a {
			font-size: 1.3rem;
			padding: 0.5rem 1rem;
		}
	}

	@media (max-width: 768px) {
		.nav-wrapper {
			height: var(--mobile-nav-height);
		}
		
		nav {
			height: var(--mobile-nav-height);
			padding: 0.5rem 0;
		}

		.nav-container {
			gap: 1rem;
		}

		.nav-links {
			gap: 1.4rem;
		}

		.nav-links a {
			font-size: 1.25rem;
		}
	}
</style>
