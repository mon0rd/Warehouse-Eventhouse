<script>
	import '/src/styles/index.sass';
	import favicon from '$lib/assets/favicon.svg';
	import Header from '$lib/components/Header.svelte';

	import { page } from '$app/state';
	import { locales, localizeHref } from '$lib/paraglide/runtime';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
	<meta name="description" content="Event Management, we Organise your events" />
	<meta name="author" content="Tetiana" />
	<title>Warehouse Eventhouse</title>
</svelte:head>

<div class="wrapper">
	<Header />
	<main>
		{@render children()}
		<div style="display:none">
			{#each locales as locale}
				<a href={localizeHref(page.url.pathname, { locale })}>
					{locale}
				</a>
			{/each}
		</div>
	</main>
</div>

<style lang="sass">
.wrapper
	margin: 0 auto
	padding: 10px 50px
	// height: 10000px
</style>
