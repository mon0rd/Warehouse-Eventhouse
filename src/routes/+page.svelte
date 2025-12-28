<script>
	const line1 = 'Mehr als ein Club...'.split('');
	const line2Text = 'Es ist HOUSE';
	const line2Chars = line2Text.split('');

	const offset = 0.3; // when first letter appears
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

<div class="main_wrapper">
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
</div>

<style lang="sass">
.main_wrapper
  display: flex
  flex-direction: column
  justify-content: center
  align-items: center
  height: 80vh
  margin-left: 30px

h1
  text-align: center
  font-size: 80px
  margin: 0
  font-weight: 500

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
</style>
