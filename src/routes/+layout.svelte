<script>
	import '/src/styles/index.sass';
	import favicon from '$lib/assets/favicon.svg';

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
	overflow: visible
	margin: 0 auto
	padding: 0 10px 10px 10px
	main
		overflow: visible
	@media screen and (min-width: 768px) 
		padding: 0 60px 10px 60px
	@media screen and (min-width: 1200px) 
		padding: 0 180px 10px 180px
	@media screen and (min-width: 1920px) 
		padding: 0 240px 10px 240px
</style>
