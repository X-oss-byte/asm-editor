<script lang="ts">
	import Page from '$cmp/layout/Page.svelte'
	import Navbar from '$cmp/Navbar.svelte'
	import NavigationLinkButton from '$cmp/NavigationLinkButton.svelte'
	import { capitalize } from '$lib/utils'
	import FaArrowRight from 'svelte-icons/fa/FaArrowRight.svelte'
	const langs = ['m68k']
</script>

<Navbar>
	<a class="icon" href="/" title="Go to the home">
		<img src="/favicon.png" alt="logo" />
		Home
	</a>
</Navbar>
<svelte:head>
	<title>Documentation</title>
	<meta
		name="description"
		content="All the documentations for the languages and features available in the asm-editor"
	/>
	<meta
		property="og:description"
		content="All the documentations for the languages and features available in the asm-editor"
	/>
	<meta name="og:title" content="Documentation" />
</svelte:head>

<Page cropped contentStyle="margin-top: 4rem; gap: 1rem; padding: 1rem">
	<h1>Languages</h1>
	<div class="langs">
		{#each ['m68k'] as lang}
			<NavigationLinkButton href="/documentation/{lang}">
				<div>
					{capitalize(lang)}
				</div>
			</NavigationLinkButton>
		{/each}
	</div>
</Page>

<style lang="scss">
	.langs {
		display: grid;
		grid-template-columns: repeat(2, minmax(10rem, 1fr));
		gap: 1rem;
		margin-left: 1rem;
	}
	h1 {
		font-size: 1.8rem;
		font-weight: normal;
	}
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		padding: 0.8rem;
		margin-top: 1rem;
		padding-top: 4rem;
	}
	.icon {
		height: 2.2rem;
		display: flex;
		align-items: center;
		gap: 1rem;
		img {
			height: 100%;
		}
		&:hover {
			color: var(--accent);
		}
	}
</style>
