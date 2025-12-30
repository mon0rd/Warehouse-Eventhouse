<!-- Events.svelte -->
<script>
	const { events, visibleDesktop = 3.2 } = $props();

	let currentIndex = $state(0);

	// width of one card in percentage of viewport
	const cardWidth = $derived(100 / visibleDesktop);
	// gap between cards in %
	const gap = 1.5;
	// how wide the whole track is
	const trackWidth = $derived(events.length * cardWidth + (events.length - 1) * gap);

	// how much the track overflows the viewport (in %)
	const overflow = $derived(Math.max(0, trackWidth - 100));

	const maxIndex = $derived(Math.max(0, events.length - Math.floor(visibleDesktop)));

	// how far we should shift for the current index
	const offset = $derived(maxIndex > 0 ? (overflow * currentIndex) / maxIndex : 0);

	function next() {
		if (currentIndex < maxIndex) currentIndex += 1;
	}

	function prev() {
		if (currentIndex > 0) currentIndex -= 1;
	}
</script>

<div class="events_wrapper">
	<h2 class="header">Bevorstehende Veranstaltungen</h2>

	<div class="slider">
		<button
			class="navButton navButton--prev"
			onclick={prev}
			disabled={currentIndex === 0}
			aria-label="Vorherige Events"
		>
			‹
		</button>

		<div class="track_wrapper">
			<div class="track" style={`transform: translateX(-${offset}%);`}>
				{#each events as event}
					<a
						href={event.link}
						class="card"
						target="_blank"
						rel="noopener noreferrer"
						style={`flex: 0 0 calc(100% / ${visibleDesktop});`}
					>
						<div class="image_wrapper">
							<img src={event.image} alt={event.title} loading="lazy" />
						</div>

						<div class="body">
							{#if event.date}
								<span class="date">{event.date}</span>
							{/if}
							<h3 class="title">{event.title}</h3>
							{#if event.location}
								<p class="location">{event.location}</p>
							{/if}
						</div>
					</a>
				{/each}
			</div>
		</div>

		<button
			class="navButton navButton--next"
			onclick={next}
			disabled={currentIndex === maxIndex}
			aria-label="Nächste Events"
		>
			›
		</button>
	</div>
</div>

<style lang="sass">
.events_wrapper
    height: 100%
    display: flex
    flex-direction: column

.header
    flex: 0 0 auto
    font-size: clamp(2.2rem, 3vw, 3.5rem)
    text-align: center

.slider
    flex: 1
    margin-top: 12vh
    display: flex
    align-items: center
    position: relative 

.track
    height: 100%
    display: flex
    gap: 1.5%
    transition: transform 0.45s cubic-bezier(0.22, 0.61, 0.36, 1)
    will-change: transform
    &_wrapper
        flex: 1 1 auto
        height: 100%
        overflow-x: hidden
        padding: 16px 0

.navButton
    position: absolute
    top: 50%
    transform: translateY(-50%)
    z-index: 5
    color: white
    border: none
    background: rgba(255, 255, 255, 0.06)
    backdrop-filter: blur(8px)
    width: 3rem
    height: 3rem
    border-radius: 999px
    font-size: 1.8rem
    display: flex
    align-items: center
    justify-content: center
    cursor: pointer
    transition: transform 0.2s ease, background 0.2s ease, opacity 0.2s ease
    &:hover:not(:disabled)
        background: rgba(255, 255, 255, 0.12)
    &:disabled
        opacity: 0.3
        cursor: default
    &--prev
      left: -4.5rem   
    &--next
      right: -4.5rem   

.card
    position: relative
    height: 100%  
    min-width: 0
    background: radial-gradient(circle at top left, rgba(255, 255, 255, 0.12), rgba(8, 0, 20, 0.95))
    border-radius: 1.2rem
    text-decoration: none
    color: inherit
    display: block
    overflow: hidden 
    box-shadow: 0 18px 40px rgba(0, 0, 0, 0.45)
    transition: transform 0.25s ease, box-shadow 0.25s ease, background 0.25s ease
    &:hover
        transform: translateY(-16px)
        box-shadow: 0 22px 50px rgba(0, 0, 0, 0.65)

.image
    &_wrapper
        position: absolute
        padding: 0.9rem 0.9rem
        inset: 0
        border-radius: 0.9rem
        img
            width: 100%
            border-radius: 1.2rem
            height: 100%
            // object-fit: cover
            display: block
            // transform: scale(1.02)
            transition: transform 0.4s ease
        .card:hover & img
            transform: scale(1.05)

.body
  position: absolute
  left: 0
  right: 0
  bottom: 0
  min-height: max-content
  padding: 1rem 1.1rem 1.2rem
  display: flex
  flex-direction: column
  align-items: flex-start
  justify-content: flex-end
  gap: 0.15rem
  background: linear-gradient(
    to top,
    rgba(0, 0, 0, 0.9) 0%,
    rgba(0, 0, 0, 0.7) 40%,
    rgba(0, 0, 0, 0.0) 100%
  )
  backdrop-filter: blur(10px)
  color: #fff
  transform: translateY(100%)
  transition: transform 0.35s cubic-bezier(0.22, 0.61, 0.36, 1)

.card:hover .body
  transform: translateY(0%)

.date
  font-size: 0.85rem
  text-transform: uppercase
  letter-spacing: 0.08em
  opacity: 0.8

.title
  font-weight: 700
  font-size: 1.1rem
  margin-top: 0.2rem

.location
  font-size: 0.9rem
  opacity: 0.85
  margin-top: 0.3rem

// --- MOBILE / TABLET: native swipe + scroll-snap ---

@media (max-width: 900px)
  .slider
    gap: 1rem

  .track
    &_wrapper
        overflow-x: auto
        overflow-y: hidden
        -webkit-overflow-scrolling: touch
        scroll-snap-type: x mandatory
        scroll-padding-inline: 1.5rem

    // hide scrollbars
    scrollbar-width: none
    -ms-overflow-style: none

    &::-webkit-scrollbar
      display: none

  .track
    transform: none !important
    padding-bottom: 0.5rem

  .card
    flex: 0 0 80%
    scroll-snap-align: start

  .navButton
    display: none
</style>
