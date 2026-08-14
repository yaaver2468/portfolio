<script lang="ts">
	interface Project {
		title: string;
		date: string;
		image: string;
		description: string;
		languages: string;
		link?: string;
	}

	interface Props {
		project: Project;
		colorDict: Record<string, string>;
	}

	let { project, colorDict }: Props = $props();
</script>

<div class="project-card" class:no-link={!project.link}>
	{#if project.link}
		<a href={project.link} title="Checkout this project" class="card-link"></a>
	{/if}
	<div class="header">
		<h2 class="title">{project.title}</h2>
		<span class="date">{project.date}</span>
	</div>

	<div class="image-container">
		<img src={project.image} alt={project.title} />
	</div>

	<div class="content">
		<section class="description">
			<p>{project.description}</p>
		</section>

		<section class="languages-container">
			{#each project.languages.split(', ') as lang}
				{@const langName = lang.trim()}
				{@const dotColor = colorDict[langName] || '#ffffff'}
				<div class="lang-tag" style="--dot-color: {dotColor}">
					<span class="dot"></span>
					<span class="lang-text">{langName}</span>
				</div>
			{/each}
		</section>
	</div>
</div>

<style>
	.project-card {
		display: flex;
		flex-direction: column;
		background-color: #3a3a3a;
		border-radius: 0.75rem;
		overflow: hidden;
		position: relative;
		transition: transform 0.2s ease-in-out;
		box-shadow: 0 6px 12px -4px rgba(38, 38, 38, 0.8);
		z-index: 1;
	}

	.project-card:hover {
		transform: translateY(-0.5rem);
	}

	.no-link {
		cursor: default;
	}

	.card-link {
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		z-index: 1;
	}

	.header {
		display: flex;
		justify-content: space-between;
		align-items: baseline;
		padding: 1rem 1rem 1rem 1rem;
	}

	.title {
		margin: 0;
		font-size: 1.25rem;
		font-weight: bold;
		color: #ffffff;
	}

	.date {
		font-size: 0.875rem;
		color: #aaa;
	}

	.image-container {
		width: 100%;
		aspect-ratio: 16 / 9;
		overflow: hidden;
	}

	.image-container img {
		width: 100%;
		height: 100%;
		object-fit: cover;
	}

	.content {
		padding: 1rem;
		display: flex;
		flex-direction: column;
		gap: 1rem;
	}

	.description {
		margin: 0;
		font-size: 1rem;
		line-height: 1.4;
		color: #e0e0e0;
	}

	.languages-container {
		display: flex;
		flex-wrap: wrap;
		gap: 0.5rem;
		z-index: 2;
	}

	.lang-tag {
		display: inline-flex;
		align-items: center;
		padding: 0.3rem 0.7rem;
		background-color: #4a4a4a;
		border-radius: 2rem;
		gap: 0.5rem;
		font-size: 0.875rem;
		transition: box-shadow 0.2s ease;
	}

	.lang-tag:hover {
		box-shadow: inset 0 0 8px var(--dot-color);
	}

	.dot {
		width: 8px;
		height: 8px;
		background-color: var(--dot-color);
		border-radius: 50%;
		box-shadow: 0 0 5px var(--dot-color);
	}

	.lang-text {
		color: #ffffff;
	}
</style>
