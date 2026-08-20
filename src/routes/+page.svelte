<script lang="ts">
	import { onMount } from "svelte";
	import { colorDict } from "$lib/constants";
	import ProjectCard from "$lib/components/ProjectCard.svelte";
	import aiGmail from "$lib/assets/projects/ai_gmail.jpg";
	import tuningByNick from "$lib/assets/projects/tuningbynick.jpg";
	import unity from "$lib/assets/projects/unity.jpg";
	import gameTracker from "$lib/assets/projects/tracker.jpg";
	import discord from "$lib/assets/projects/discord.jpg";
	import twitch from "$lib/assets/projects/twitch.jpg";
	import streamDeck from "$lib/assets/projects/stream-deck-1.jpg";
	import challahland from "$lib/assets/projects/challah-1.jpg";

	interface Project {
		title: string;
		date: string;
		image: string;
		description: string;
		languages: string;
		link?: string;
	}

	let projects: Project[] = [
		{
			title: "AI Email Automation",
			date: "Aug 2026",
			image: aiGmail,
			description: "A small business was using Claude to generate drafts for the emails that were coming in from their customers. However the quantity of incoming emails was overwhelming what they could promptly respond to and Claude's basic integration with Gmail wasn't properly drafting emails as they came in. An elegant solution that worked for them with no additional costs was to use the Gmail API to connect to the Claude SDK and create drafts promptly.",
			languages: "Node.js, Gmail API, Claude SDK",
		},
		{
			title: "Shopify Site Redesign",
			date: "Sep 2025",
			image: tuningByNick,
			description: "Nick runs a business doing car-tunes and selling custom auto-performance parts. His business is hosted on the Shopify ecommerce platform and he was lookging for a full site redesign and content reorganization.",
			languages: "Liquid, HTML, Javascript, CSS",
			link: "https://tuningbynick.com/"
		},
		{
			title: "Shopify Site Redesign",
			date: "Dec 2022",
			image: unity,
			description: "Unity Performance is a company that sells car performance parts, currently focusing on parts for Honda & Acura but with ambitions to grow into many other brands that their consumers need.\nThey had their previous website layout since they first launched on Shopify about 3 years ago and were looking for a total revamp plus some help with cleaning out extra Shopify apps which were slowing down the site itself. The new site design along with optimizations helped to create a 135% increase in the speed score provided by Shopify. Furthermore I was able to remove extra apps by coding in the functionaltiies myself, which allowed for Unity to save ~$30/month on their billing cycles.\nStill their active developer and site maintainer as of 2026.",
			languages: "Liquid, HTML, Javascript, CSS",
			link: "https://unity-performance.com/"
		},
		{
			title: "Game Tracker",
			date: "Oct 2018",
			image: gameTracker,
			description: "Built to read the replay files generated from the popular game Fortnite. This program parses the data from the generated files post-match and relays everything on the Electron front end.\n\nWhat initially started out as a fun idea running through a terminal in 2018 for one of the video games I played eventually turned into a project that I was able to scale up into multi-functional desktop application. At the time of this post the project is currently in its 3rd revision and available for free to others who play as well.\nAt the peak of my activity with this project I had connected with an online entertainer who currently has over 6 million followers and made it a part of his daily routine to use my program. This allowed for me to practice a developer-consumer feedback cycle which was a great learning experience that contributed to the current state and functionality of this open-source program.",
			languages: "Vue, C#, Electron, Python, AHK",
			link: "https://github.com/yaaver2468/Game-Tracker"
		},
		{
			title: "Discord Bot",
			date: "Apr 2019",
			image: discord,
			description: "Joining discord in the first year that it launched and watching it evolve and takeover the messaging tech-space has been an exciting journey. Summer of 2019 I decided to start tinkering with the API a little bit to setup a test bot on my discord server and practice the functionalities of Discord. I continue to host the bot to this day for friends that need a simple discord bot with user management features, voice-text channels (one of the first to create this feature before discord made it official), twitch notifications system, and anything else that my users suggest.",
			languages: "Node.JS, Discord API",
		},
		{
			title: "Twitch Bot",
			date: "Dec 2020",
			image: twitch,
			description: "Working as a community-manager in an online community for a popular video game streamer was the insipiration for me to create this bot application. Having a firm grasp of REST API's with my work on my personal discord bot, building the twitch bot as my secondary project was quite a simple task.\n\nCreating chat filters that were tried and tested to correctly purge users unable to follow community guidelines was the primary task of this bot. Utilizing regex patterns, message history, and integrations with discord, I was able to make a bot that enforced community guidelines with over 60,000 moderation events with less than 2% false-positives.\n\nThe database for this currently stores user information for over 12,000 unique users and more than 6,000,000 messages which can be accessed by staff members (or other authorized users) at any point as well for moderation purposes.",
			languages: "Node.JS, Twitch API",
		},
		{
			title: "Stream Deck",
			date: "Jun 2019",
			image: streamDeck,
			description: "Stark is an online entertainer who was looking for a program that would simplify all the programs he has to manage for his entertainment system. With over 7-12 different applications running simultaneously that he would have to navigate through on a daily basis.\n\nAt the time Stark was considering getting the Elgato stream deck which would make quick work of many of the redundant actions he had to take. However, this professional solution did not have certain custom features that he wanted. He asked me to create a software solution that could replicate the functionality of the physical hardware he was considering. The software solution was to be created in such a way that he could customize it themselves in the future with his knowledge of AHK. Lightweight, simple to use, and with practically an infinite number of assignable buttons, this simple program became his daily driver to manage all his tasks from one GUI interface.",
			languages: "AHK",
			link: "https://github.com/yaaver2468/Stream-Deck"
		},
		{
			title: "Shopify Site Customization",
			date: "Jul 2021",
			image: challahland,
			description: "The client is a bakery based in Montreal that sells special bread called Challah. However, at the time their site was missing a key component that was required for every order, a pickup date. They found solutions from the Shopify app store that they could potentially use, but all of them were missing the time specific constraints that they wished to have with their orders.\nThe integration I created for their site was more than just simple hard-set code which the client would never be able to adjust in the future. With my knowledge of Liquid I was able to integrate the order-pickup-time component seamlessly into Shopify so that they could access it through their theme editor for any future adjustments to their schedule. This integration was exactly what they needed for their orders and it made the order fullfillment cycle much faster having eliminated the need to contact customers to arrange pickups.",
			languages: "Liquid, HTML, Javascript, CSS",
			link: "https://www.instagram.com/challahland_mtl/"
		},

	];

	let numColumns = $state(window.matchMedia("(min-width: 768px)").matches ? 3 : 1);

	onMount(() => {
		const mql = window.matchMedia("(min-width: 768px)");
		const updateColumns = () => {
			numColumns = mql.matches ? 3 : 1;
		};
		mql.addEventListener("change", updateColumns);
		return () => mql.removeEventListener("change", updateColumns);
	});

	let columns = $derived.by(() => {
		const cols: Project[][] = Array.from({ length: numColumns }, () => []);
		projects.forEach((project, i) => {
			cols[i % numColumns].push(project);
		});
		return cols;
	});
</script>

<header class="hero">
	<h1 class="name">Yaaver Imran</h1>
	<h2 class="title">Fullstack Developer | Systems Consultant</h2>
	<p class="about">
		I'm a fullstack engineer with a strong foundation in cloud infrastructure and technical delivery. I combine modern web development with deep systems administration experience to build scalable applications and custom web solutions.
		<br/>
		Interested in end-to-end fullstack development, application modernization, or custom infrastructure design? <a href="/contact">Let's connect</a>.
	</p>
</header>

<h1 class="page-title">My Projects</h1>
<h5 style="margin-top: 0;">outlined projects have a link</h5>

<div class="project-columns">
	{#each columns as column}
		<div class="project-column">
			{#each column as project}
				<ProjectCard {project} {colorDict} />
			{/each}
		</div>
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
		color: white;
	}

	.title {
		font-size: 2rem;
		margin: 0;
		color: color-mix(in srgb, var(--global-white) 60%, var(--global-bg));
	}

	.about {
		font-size: 1.25rem;
		line-height: 1.6;
		max-width: 800px;
	}

	.page-title {
		font-size: 2rem;
		margin-bottom: 1rem;
	}

	.project-columns {
		display: flex;
		gap: 2rem;
	}

	.project-column {
		flex: 1 1 0;
		min-width: 0;
		display: flex;
		flex-direction: column;
		gap: 2rem;
	}
</style>