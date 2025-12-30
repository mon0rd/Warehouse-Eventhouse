<script>
	import { onMount } from 'svelte';

	let visible = false;
	let viewportHeight = 0;

	// how many "screen heights" to scroll before showing
	const thresholdFactor = 2; // 300vh

	function handleScroll() {
		if (!viewportHeight) return;
		visible = window.scrollY > viewportHeight * thresholdFactor;
	}

	function updateViewportHeight() {
		viewportHeight = window.innerHeight;
		handleScroll();
	}

	function scrollToTop() {
		window.scrollTo({
			top: 0,
			behavior: 'smooth'
		});
	}

	onMount(() => {
		updateViewportHeight();
		window.addEventListener('scroll', handleScroll);
		window.addEventListener('resize', updateViewportHeight);

		return () => {
			window.removeEventListener('scroll', handleScroll);
			window.removeEventListener('resize', updateViewportHeight);
		};
	});
</script>

<button class="scroll_top" onclick={scrollToTop} aria-label="Nach oben scrollen" class:visible>
	<svg
		class="icon"
		viewBox="0 0 24 24"
		fill="none"
		stroke="currentColor"
		stroke-width="1"
		stroke-linecap="round"
		stroke-linejoin="round"
		aria-hidden="true"
	>
		<polyline points="18 15 12 9 6 15" />
	</svg>
</button>

<style lang="sass">
.scroll_top
	position: fixed
	right: 2rem
	bottom: 2rem
	width: 3rem
	height: 3rem
	border-radius: 999px
	border: none
	cursor: pointer
	display: flex
	align-items: center
	justify-content: center
	background: rgba(255, 255, 255, 0.06)
	backdrop-filter: blur(8px)
	color: white
	opacity: 0
	pointer-events: none
	transform: translateY(8px)
	transition: opacity .25s ease, transform .25s ease, background .2s ease
	z-index: 50

	&.visible
		opacity: 1
		pointer-events: auto
		transform: translateY(0)

	&:hover
		background: rgba(255, 255, 255, 0.1)

.icon
	width: 1.4rem
	height: 1.4rem
	fill: currentColor

@media (max-width: 600px)
	.scroll_top
		right: 1.2rem
		bottom: 1.2rem
		width: 2.6rem
		height: 2.6rem
		.icon
			width: 1.2rem
			height: 1.2rem
</style>
