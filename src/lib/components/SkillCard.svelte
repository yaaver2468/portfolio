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

<div class="card">
	<div class="header">
		<h2 class="category">{group.category}</h2>
	</div>

	<div class="card-content">
		<div class="card-tags">
			{#each group.skills.split(",") as skill}
				{@const dotColor = colorDict[skill.trim().toLowerCase()] || "var(--global-white)"}

				<div class="blip-tag" style="--dot-color: {dotColor}">
					<span class="dot"></span>
					<span class="skill-text">{skill.trim()}</span>
				</div>
			{/each}
		</div>
	</div>
</div>

<style>
	.header {
		padding: 1rem 1rem 0 1rem;
	}
	.category {
		margin: 0;
		font-size: 1.25rem;
		font-weight: bold;
	}
</style>