<script lang="ts">
	import Arrow from "$lib/assets/arrow.svg?component";
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

	const DESCRIPTION_LIMIT = 40;
	let expanded = $state(false);

	let isLongDescription = $derived(project.description.length > DESCRIPTION_LIMIT);
	let displayedDescription = $derived(
		expanded || !isLongDescription
			? project.description.replaceAll("\n", "<br/>")
			: project.description.slice(0, DESCRIPTION_LIMIT).trimEnd() + "…"
	);

	function toggleExpanded(e: MouseEvent) {
		e.preventDefault();
		e.stopPropagation();
		expanded = !expanded;
	}
</script>

<div class="project-card" class:no-link={!project.link}>
	{#if project.link}
		<a href={project.link} title="Checkout this project" class="card-link" target="_blank"></a>
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
			<!-- svelte-ignore a11y_click_events_have_key_events -->
			<!-- svelte-ignore a11y_no_noninteractive_element_interactions -->
			<p
				class:clickable={isLongDescription}
				onclick={isLongDescription ? toggleExpanded : undefined}
			>{@html displayedDescription}</p>
			{#if isLongDescription}
				<button
					type="button"
					class="expand-toggle"
					class:expanded
					onclick={toggleExpanded}
					aria-expanded={expanded}
					aria-label={expanded ? "Show less" : "Show more"}
				>
					<Arrow />
				</button>
			{/if}
		</section>
		<section class="languages-container">
			{#each project.languages.split(",") as lang}
				{@const langName = lang.trim()}
				{@const dotColor = colorDict[langName] || "var(--global-white)"}
				<div class="blip-tag" style="--dot-color: {dotColor}">
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
		background-color: rgba(58, 58, 58, 0.6);
		border-radius: 0.75rem;
		overflow: hidden;
		position: relative;
		transition: transform 0.2s ease-in-out;
		box-shadow: 0 6px 12px -4px color-mix(in srgb, var(--global-white) 20%, transparent);
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
	}
	.date {
		font-size: 0.875rem;
		color: color-mix(in srgb, var(--global-white) 60%, transparent);
		min-width: 64px;
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
		position: relative;
		z-index: 2;
		display: flex;
		flex-direction: column;
		gap: 0.5rem;
		margin: 0;
		font-size: 1rem;
		line-height: 1.4;
	}

	.description p.clickable {
		cursor: pointer;
	}

	.expand-toggle :global {
		align-self: flex-start;
		display: inline-flex;
		align-items: center;
		justify-content: center;
		background: none;
		border: none;
		color: var(--global-white);
		cursor: pointer;
		padding: 0.25rem;
		border-radius: 50%;
		transition: background-color 0.2s ease;
		svg {
			transition: transform 0.2s ease;
		}
	}

	.expand-toggle:hover {
		background-color: color-mix(in srgb, var(--global-white) 15%, transparent);
	}

	.expand-toggle.expanded :global {
		svg {
			transform: rotate(180deg);
		}
	}

	.languages-container {
		display: flex;
		flex-wrap: wrap;
		gap: 0.5rem;
		z-index: 2;
	}
</style>