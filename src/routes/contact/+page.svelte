<script lang="ts">
	const SUBTEXT_TIMEOUT = 1500;

	let email = "admin[at]yaaverimran[dot]com";
	let timeoutTrack = -1;
	let subtext = $state("click to copy");

	async function copyToClipboard() {
		try {
			const realEmail = "admin@yaaverimran.com";
			await navigator.clipboard.writeText(realEmail);
			subtext = "copied to clipboard!";
		} catch (err) {
			console.error("Failed to copy: ", err);
			subtext = "failed to copy";
		}
		if (timeoutTrack != -1) {
			clearTimeout(timeoutTrack);
		}
		timeoutTrack = setTimeout(() => {
			subtext = "click to copy";
			timeoutTrack = -1;
		}, SUBTEXT_TIMEOUT);
	}
</script>

<div class="contact-container">
	<button class="email-text" onclick={copyToClipboard}>
		{email}
	</button>
	<p class="subtext">{subtext}</p>
</div>

<style>
	.contact-container {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		min-height: 50vh;
		gap: 0.5rem;
	}

	.email-text {
		background: none;
		border: none;
		padding: 0;
		font-size: 2.5rem;
		color: var(--global-white);
		cursor: pointer;
		font-family: inherit;
		transition: color 0.2s ease;
	}

	.email-text:hover {
		color: white;
	}

	.subtext {
		font-size: 1rem;
		color: color-mix(in srgb, var(--global-white) 50%, transparent);
		margin: 0;
	}
</style>
