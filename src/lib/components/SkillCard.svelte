<script lang="ts">
	import { colorDict } from "$lib/constants";

	interface SkillGroup {
		category: string;
		skills: string;
	}

	interface Props {
		group: SkillGroup;
	}

	let { group }: Props = $props();
</script>

<div class="skill-card">
	<div class="header">
		<h2 class="category">{group.category}</h2>
	</div>

	<div class="content">
		<div class="skills-container">
			{#each group.skills.split(",") as skill}
				{@const dotColor = colorDict[skill.trim()] || "var(--global-white)"}

				<div class="blip-tag" style="--dot-color: {dotColor}">
					<span class="dot"></span>
					<span class="skill-text">{skill.trim()}</span>
				</div>
			{/each}
		</div>
	</div>
</div>

<style>
	.skill-card {
		display: flex;
		flex-direction: column;
		background-color: rgba(58, 58, 58, 0.6);
		border-radius: 0.75rem;
		overflow: hidden;
		position: relative;
		transition: transform 0.2s ease-in-out;
		box-shadow: 0 6px 12px -4px
			color-mix(in srgb, var(--global-white) 20%, transparent);
	}

	.skill-card:hover {
		transform: translateY(-0.5rem);
	}

	.header {
		padding: 1rem 1rem 0 1rem;
	}

	.category {
		margin: 0;
		font-size: 1.25rem;
		font-weight: bold;
	}

	.content {
		padding: 1rem;
		display: flex;
		flex-direction: column;
		gap: 1rem;
	}

	.skills-container {
		display: flex;
		flex-wrap: wrap;
		gap: 0.5rem;
	}
</style>