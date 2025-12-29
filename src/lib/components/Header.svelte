<script>
	import { onMount } from 'svelte';
	import { fade } from 'svelte/transition';

	let open = false;
	let mounted = false;
	const links = ['History', 'Events', 'Impressum', 'Kontakt'];

	let headerEl;

	function handleDocumentClick(event) {
		if (!open || !headerEl) return;

		const target = event.target;

		// elements that should NOT close the menu when clicked
		const insideHeader = headerEl.contains(target);
		const insideMenu = target.closest('.menu');
		const insideNav = target.closest('.nav');
		const insideLogo = target.closest('.main_logo');

		if (!insideHeader) {
			open = false;
			return;
		}

		if (!insideMenu && !insideNav && !insideLogo) {
			open = false;
		}
	}

	function handleScroll() {
		if (open) open = false;
	}

	function handleKeydown(event) {
		if (!open) return;
		if (event.key === 'Escape' || event.key === 'Esc') {
			open = false;
		}
	}

	onMount(() => {
		mounted = true;
		document.addEventListener('click', handleDocumentClick);
		window.addEventListener('scroll', handleScroll);
		window.addEventListener('keydown', handleKeydown);
		return () => {
			document.removeEventListener('click', handleDocumentClick);
			window.removeEventListener('scroll', handleScroll);
			window.removeEventListener('keydown', handleKeydown);
		};
	});
</script>

{#if mounted}
	<header class="header" class:open bind:this={headerEl} in:fade={{ duration: 600 }}>
		<button
			type="button"
			class="logo_button"
			aria-label="Toggle menu"
			aria-expanded={open}
			onclick={() => (open = !open)}
		>
			<img src="/images/MainLogo.jpg" alt="Warehouse Eventhouse logo" class="main_logo" />
		</button>

		<button
			class="nav"
			type="button"
			onclick={() => (open = !open)}
			aria-expanded={open}
			aria-label="Navigation"
		>
			<div class="nav_bar"></div>
			<div class="nav_bar"></div>
			<div class="nav_bar"></div>
		</button>

		<div class="menu" aria-hidden={!open}>
			{#each links as link}
				<a href="/" class="menu_link">{link}</a>
			{/each}
		</div>
	</header>
{/if}

<style lang="sass">
.header  
  padding-top: 10px
  height: var(--header-h)
  position: absolute
  top: 0
  left: 0
  right: 0
  display: flex
  flex-direction: column
  align-items: center
  z-index: 100
  // control card + border size:
  --menu-width: 122.5px
  --menu-height: 150px
  --menu-top-offset: 22px

.logo_button
  padding: 0
  border: none
  background: none
  cursor: pointer
  display: inline-flex
  align-items: center
  justify-content: center
  &:focus-visible
    outline: none

.main_logo
  height: 60px
  border-radius: 15px
  display: block
  z-index: 5

.nav
  position: relative
  margin-top: 8px
  display: flex
  flex-direction: column
  align-items: center
  gap: 5px
  padding: 8px 0
  height: 16px
  width: 60px
  background: transparent
  border: none
  cursor: pointer
  z-index: 1

.nav_bar
  position: absolute
  left: auto
  width: 60px
  height: 2px
  background-color: white
  border-radius: 999px  
  transform-origin: center
  transition: all .65s cubic-bezier(.19, 1, .22, 1)

.nav_bar:nth-child(3)
  top: 16px
  left: 50%
  transform: translate(-50%, 0) scaleX(1)

.menu
  position: absolute
  top: var(--menu-top-offset)
  left: 50%
  width: var(--menu-width)
  height: var(--menu-height)
  transform: translateX(-50%) translateY(-10px) scale(.9)
  display: flex
  flex-direction: column
  align-items: center
  justify-content: center
  padding: 40px 28px 0 28px
  background: #ffffff
  border-radius: 22px
  opacity: 0
  pointer-events: none
  transition: opacity .25s ease-out, transform .25s ease-out
  z-index: 2

.menu_link
  position: relative
  display: block
  padding: 6px 0
  color: #000
  text-decoration: none
  font-size: 16px
  font-weight: 400
  text-align: center
  width: 100%

.menu_link + .menu_link::before
  content: ''
  position: absolute
  top: 0
  left: 0
  right: 0
  height: 1px
  background: #000
  opacity: .15

.header.open .menu
  opacity: 1
  transform: translateX(-50%) translateY(0) scale(1)
  pointer-events: auto
  z-index: 2  

.header.open .nav_bar
  background: #ffffff
  border-radius: 0
  z-index: 1    
  
.nav_bar:nth-child(1)
  top: 0

.nav_bar:nth-child(2)
  top: 8px

.nav_bar:nth-child(3)
  top: 16px

.header.open .nav_bar:nth-child(1)
  /* left vertical border */
  top: calc(var(--menu-top-offset) - 66px)
  left: 51%
  width: 2px
  height: calc(var(--menu-height) + 10px)
  transform: translateX(calc(-1 * var(--menu-width) / 2 - 24px))

.header.open .nav_bar:nth-child(2)
  /* right vertical border */
  top: calc(var(--menu-top-offset) - 66px)
  left: 48%
  width: 2px
  height: calc(var(--menu-height) + 10px)
  transform: translateX(calc(1 * var(--menu-width) / 2 + 24px))

.header.open .nav_bar:nth-child(3)
  width: var(--menu-width)
  transform: translate(-50%, 114px) scaleX(1.2)

@media (prefers-reduced-motion: reduce)
  .nav_bar,
  .menu
    transition: none

</style>
