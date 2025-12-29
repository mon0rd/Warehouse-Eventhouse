<script>
	import Header from '$lib/components/Header.svelte';

	const line1 = 'Mehr als ein Club...'.split('');
	const line2Text = 'Es ist HOUSE';
	const line2Chars = line2Text.split('');

	const offset = 0.8; // when first letter appears
	const baseGap = 0.1; // minimum gap between letters
	const curveStrength = 1.3; // how much slower towards the end

	const delays = line1.map((_, i) => {
		const t = i / (line1.length - 1);
		const curved = t * t;
		return offset + i * baseGap + curved * curveStrength;
	});

	const line2Delay = delays[delays.length - 1] + 1.2;

	const houseStart = line2Text.indexOf('HOUSE');
	const houseEnd = houseStart + 'HOUSE'.length;
</script>

<section class="hero">
	<Header />
	<h1>
		{#each line1 as char, i}
			<span class="char line1char" style={`--delay:${delays[i]}s`}>
				{char === ' ' ? '\u00A0' : char}
			</span>
		{/each}

		<br />

		{#each line2Chars as char, i}
			{#if i >= houseStart && i < houseEnd}
				<span class="char line2char" style={`--delay:${line2Delay}s`}>
					<span class="house">
						{char === ' ' ? '\u00A0' : char}
					</span>
				</span>
			{:else}
				<span class="char line2char" style={`--delay:${line2Delay}s`}>
					{char === ' ' ? '\u00A0' : char}
				</span>
			{/if}
		{/each}
	</h1>
</section>

<section class="pastandfuture">
	<div class="pastandfuture_wrapper">
		<h2 class="pastandfuture_title">Our Past & Future</h2>
		<p class="pastandfuture_descr">
			Seit vielen Jahren war das Warehouse weit mehr als nur ein Club: Es war ein kultureller
			Treffpunkt, eine Bühne für Live-Musik, Partys und besondere Momente. Hier haben Generationen
			Musik entdeckt, Freundschaften geschlossen und Traditionen geprägt. Jetzt beginnt ein neues
			Kapitel unter neuem Namen: Warehouse Eventhouse. Die Türen öffnen sich wieder — mit demselben
			offenen Geist und einer klaren Botschaft: Hier ist jede*r willkommen. Neben Konzerten und
			kulturellen Veranstaltungen wird es auch altersgerechte, sichere Events für junge Menschen,
			Rock-Shows, Themenabende und viele neue Formate geben. Und natürlich bleiben die Klassiker
			bestehen — als lebendige Erinnerung an das gute alte Warehouse.
			<br />
			<br />
			<br />
			Gleichzeitig ist das Warehouse Eventhouse ein Ort für neue Ideen und neue Veranstalter*innen. Wer
			sich kreativ ausdrücken möchte, wer Kultur, Musik oder besondere Formate auf die Bühne bringen will
			— findet hier einen Platz und offene Ohren. Die Tradition bleibt. Die Energie entwickelt sich weiter.
			Und das Warehouse Eventhouse wird mehr denn je zu einem Ort für alle Stile, alle Generationen und
			alle Geschichten.
		</p>
	</div>
</section>

<style lang="sass">
.hero
  scroll-snap-align: center
  scroll-snap-stop: always
  display: flex
  flex-direction: column
  justify-content: center
  align-items: center
  min-height: 100vh
  margin-left: 30px

h1
  text-align: center
  font-size: 80px
  margin: auto 0 auto 0
  font-weight: 400

.char
  display: inline-block
  opacity: 0
  transform: translateY(16px)
  animation: fadeUp 0.6s ease-out forwards
  animation-delay: var(--delay)

.line1char
  transform: none
  animation: fadeOnly 0.6s ease-out forwards
  animation-delay: var(--delay)

.line2char
  transform: translateY(16px)
  animation-duration: 0.8s

.house
  display: inline-block
  animation: housePulse 1.2s cubic-bezier(0.23, 1, 0.32, 1) infinite alternate
  animation-delay: calc(var(--delay) + 0.8s)

@keyframes fadeUp
  from
    opacity: 0
    transform: translateY(16px)
  to
    opacity: 1
    transform: translateY(0)

@keyframes housePulse
  0%
    transform: scale(1)
    text-shadow: 0 0 0 rgba(255, 255, 255, 0.2)
  50%
    transform: scale(1.08)
    text-shadow: 0 0 18px rgba(255, 255, 255, 0.6)
  100%
    transform: scale(1)
    text-shadow: 0 0 4px rgba(255, 255, 255, 0.4)

@keyframes fadeOnly
  from
    opacity: 0
  to
    opacity: 1

@media (prefers-reduced-motion: reduce)
  .char,
  .line2char,
  .house
    animation: none
    opacity: 1
    transform: translateY(0)
    text-shadow: none

.pastandfuture
  scroll-snap-align: center
  scroll-snap-stop: always
  position: relative 
  height: 97vh
  margin: 1.5vh 0  
  background: url("/images/PastAndFutureBG.jpg") no-repeat center center / cover
  overflow: hidden
  display: flex
  justify-content: center
  align-items: start
  &_wrapper
    position: relative
    padding: 2rem 3rem 
  &::before
    content: ''
    position: absolute
    inset: 0
    pointer-events: none
    background: linear-gradient(180deg, #000000 0%, #0a0013 100%)
    -webkit-mask-image: radial-gradient(circle at center,
      transparent -45%,
      black 100%
    )
    mask-image: radial-gradient(circle at center,
      transparent -45%,
      black 100%
    )
    opacity: 1   
  &_title
    font-size: clamp(2.2rem, 3vw, 3.5rem)
    text-align: center
  &_descr
    text-align: center
    font-size: clamp(1rem, 1.3vw, 1.2rem)
    margin-top: 9rem

</style>
