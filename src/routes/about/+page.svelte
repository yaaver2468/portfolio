<script lang="ts">
	import SkillCard from "$lib/components/SkillCard.svelte";
	import headshot from "$lib/assets/headshot.jpg";

	interface SkillGroup {
		category: string;
		skills: string;
	}

	const skills: SkillGroup[] = [
		{
			category: "Frontend",
			skills: "Svelte, TypeScript, CSS, HTML"
		},
		{
			category: "Backend",
			skills: "Node.js, Rust, Python, SQL"
		},
		{
			category: "Cloud & Tools",
			skills: "AWS, Vite, Swag"
		}
	];
</script>

<div class="about-container">
	<div class="about-content">
		<header class="about-header">
			<h1>Yaaver Imran</h1>
		</header>

		<section class="about-text">
			<p>
				My name is Yaaver Imran and I'm a versatile computer science programmer with experience in various programming languages! Thank you for viewing my Portfolio, this site was created with React + Tailwind and hosted on AWS.
				<br/><br/>
				I'm dedicated to creating innovative solutions that solve real-world problems. With experience spanning web applications, infrastructure automation, and system optimization, I thrive on tackling complex technical challenges and delivering measurable results. I maintain a commitment to continuous learning through personal projects, industry collaboration, and staying current with emerging technologies and best practices.
				What drives me is the intersection of technical excellence and user experience; whether I'm optimizing database queries for performance gains, architecting scalable infrastructure, or crafting intuitive interfaces. I believe in the power of clean code, thoughtful design, and collaborative problem-solving to create solutions that truly make a difference.
				<br/>
				When I'm not coding, you'll find me exploring new frameworks, contributing to open-source projects, or mentoring fellow developers. I'm always eager to connect with like-minded professionals and explore opportunities where I can contribute my skills while continuing to grow as a developer.
			</p>
		</section>
	</div>

	<div class="about-image">
		<img src={headshot} alt="Yaaver Imran" />
	</div>
</div>

<h1 class="page-title">Skills</h1>

<div class="skill-grid">
	{#each skills as group}
		<SkillCard {group} />
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

	.skill-grid {
		display: grid;
		gap: 2rem;
		grid-template-columns: 1fr;
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

		.skill-grid {
			grid-template-columns: repeat(3, 1fr);
		}
	}
</style>