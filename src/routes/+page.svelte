<script lang="ts">
	import ProjectCard from '$lib/components/ProjectCard.svelte';

	interface Project {
		title: string;
		date: string;
		image: string;
		description: string;
		languages: string;
		link?: string;
	}

	const colorDict: Record<string, string> = {
		'Svelte': '#ff3e00',
		'TypeScript': '#3178c6',
		'Vite': '#646cff',
		'CSS': '#41b883',
		'HTML': '#e34f26',
		'JavaScript': '#f1ca3a',
		'Node.js': '#339933',
		'Rust': '#dea584'
	};

	let projects: Project[] = [
		{
			title: 'Project One',
			date: 'Jan 2024',
			image: 'https://placehold.co/400x225',
			description: 'This is a description for project one.',
			languages: 'Svelte, TypeScript, Vite',
			link: '#'
		},
		{
			title: 'Project Two',
			date: 'Dec 2023',
			image: 'https://placehold.co/400x225',
			description: 'This is a description for project two.',
			languages: 'CSS, HTML',
		},
		{
			title: 'Project Three',
			date: 'Nov 2023',
			image: 'https://placehold.co/400x225',
			description: 'This is a description for project three.',
			languages: 'JavaScript, Node.js'
		}
	];
</script>

<header class="hero">
	<h1 class="name">Yaaver Imran</h1>
	<h2 class="title">Software developer</h2>
	<p class="about">
		Welcome to my portfolio. I am a passionate software developer dedicated to building 
		high-quality, performant, and user-centric web applications using modern technologies. 
		Explore my projects below to see my work in action.
	</p>
</header>

<h1 class="page-title">My Projects</h1>

<div class="project-grid">
	{#each projects as project}
		<ProjectCard {project} {colorDict} />
	{/each}
</div>

<style>
	.hero {
		display: flex;
		flex-direction: column;
		gap: 0.5rem;
		margin-bottom: 4rem;
		text-align: left;
	}

	.name {
		font-size: 3rem;
		margin: 0;
		color: #ffffff;
	}

	.title {
		font-size: 2rem;
		margin: 0;
		color: #aaa;
	}

	.about {
		font-size: 1.25rem;
		line-height: 1.6;
		color: #ddd;
		max-width: 800px;
	}

	.page-title {
		font-size: 2rem;
		margin-bottom: 1rem;
	}

	.project-grid {
		display: grid;
		gap: 2rem;
		grid-template-columns: 1fr;
	}

	@media (min-width: 768px) {
		.project-grid {
			grid-template-columns: repeat(3, 1fr);
		}
	}
</style>
