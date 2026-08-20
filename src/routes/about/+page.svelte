<script lang="ts">
	import SkillCard from "$lib/components/SkillCard.svelte";
	import headshot from "$lib/assets/headshot.jpg";
	import { onMount } from "svelte";

	interface SkillGroup {
		category: string;
		skills: string;
	}

	const skills: SkillGroup[] = [
		{
			category: "Languages",
			skills: "C#, Go, Java, JavaScript, Kotlin, PHP, Python, Ruby, Rust, Scala, SQL, T-SQL, TypeScript",
		},
		{
			category: "Frontend",
			skills: "Angular, Bootstrap, CSS, HTML, Next.js, React, SCSS, Shopify Liquid, Svelte, Tailwind CSS, Vue",
		},
		{
			category: "Backend & APIs",
			skills: "API, GraphQL, Node.js, Rails",
		},
		{
			category: "Databases",
			skills: "MongoDB, MySQL, PostgreSQL, Redis, SQLite",
		},
		{
			category: "Cloud & DevOps",
			skills: "AWS, Docker, Firebase, Git",
		},
		{
			category: "Design & Build Tools",
			skills: "Figma, Vite",
		},
	];

	let numColumns = $state(
		window.matchMedia("(min-width: 768px)").matches ? 3 : 1,
	);

	onMount(() => {
		const mql = window.matchMedia("(min-width: 768px)");
		const updateColumns = () => {
			numColumns = mql.matches ? 3 : 1;
		};
		mql.addEventListener("change", updateColumns);
		return () => mql.removeEventListener("change", updateColumns);
	});

	let columns = $derived.by(() => {
		const cols: SkillGroup[][] = Array.from(
			{ length: numColumns },
			() => [],
		);
		skills.forEach((group, i) => {
			cols[i % numColumns].push(group);
		});
		return cols;
	});
</script>

<div class="about-container">
	<div class="about-content">
		<header class="about-header">
			<h1>Yaaver Imran</h1>
		</header>

		<section class="about-text">
			<p>
				I'm a full-stack developer who builds web applications and handles the infrastructure behind them.
				I'm always looking to take on new challenges and learn something new.
				<br />
				Across fullstack engineering and project management roles, I've modernized
				legacy platforms, optimized high-concurrency database workloads,
				and delivered end-to-end infrastructure and application deployments.
				My experience spans logistics SaaS, live event ticketing, e-commerce,
				and custom software development.
				<br />
				I hold an Honours BSc in Computer Science, combining strong software
				engineering fundamentals with hands-on experience across the modern technology stacks outlined below.
			</p>
		</section>
	</div>

	<div class="about-image">
		<img src={headshot} alt="Yaaver Imran" />
	</div>
</div>

<h1 class="page-title">Skills</h1>

<div class="skill-columns">
	{#each columns as column}
		<div class="skill-column">
			{#each column as group}
				<SkillCard {group} />
			{/each}
		</div>
	{/each}
</div>

<style>
	.about-container {
		display: flex;
		flex-direction: column;
		gap: 2rem;
		margin-bottom: 4rem;
	}

	.about-content {
		flex: 1;
	}

	.about-image {
		flex: 0 0 auto;
		align-self: center;
	}

	.about-image img {
		width: 100%;
		height: auto;
		border-radius: 0.75rem;
		max-height: 150px;
	}

	.about-header h1 {
		font-size: 3rem;
		margin-bottom: 1rem;
	}

	.about-text {
		font-size: 1.1rem;
		line-height: 1.6;
		max-width: 800px;
	}

	.page-title {
		font-size: 2rem;
		margin-bottom: 1.5rem;
	}

	.skill-columns {
		display: flex;
		gap: 2rem;
	}

	.skill-column {
		flex: 1 1 0;
		min-width: 0;
		display: flex;
		flex-direction: column;
		gap: 2rem;
	}

	@media (min-width: 768px) {
		.about-container {
			flex-direction: row;
			justify-content: space-between;
			align-items: flex-start;
		}

		.about-image img {
			max-height: 256px;
		}
	}
</style>
