<script>
	import { onMount, onDestroy } from 'svelte';

	const MAX_FLAKES = 240;
	const SPAWN_EVERY_MS = 130;

	let snowflakes = [];
	let nextId = 0;
	let spawnInterval;

	function createFlake(id) {
		const duration = 14 + Math.random() * 8; // 10–18s per fall
		const size = 8 + Math.random() * 10; // px

		// spin between 3–8s, random direction
		const spinDuration = 3 + Math.random() * 5;
		const spinDirection = Math.random() > 0.5 ? 'normal' : 'reverse';

		// smaller flakes -> more blur
		let blur = 0;
		if (size < 11) blur = 2;
		else if (size < 14) blur = 1;
		else blur = 0;
		let opacity = 0.3 + Math.random() * 0.6;

		if (size < 11)
			opacity *= 0.8; // tiny flakes slightly fainter
		else if (size > 15) opacity = 1.0;
        
		return {
			id,
			left: Math.random() * 100, // vw
			duration,
			size,
			opacity,
			spinDuration,
			spinDirection,
			blur
		};
	}

	function removeFlake(id) {
		snowflakes = snowflakes.filter((f) => f.id !== id);
	}

	onMount(() => {
		spawnInterval = setInterval(() => {
			if (snowflakes.length >= MAX_FLAKES) return;
			const flake = createFlake(nextId++);
			snowflakes = [...snowflakes, flake];
		}, SPAWN_EVERY_MS);
	});

	onDestroy(() => {
		if (spawnInterval) clearInterval(spawnInterval);
	});
</script>

<div class="snow">
	{#each snowflakes as flake (flake.id)}
		<div
			class="snowflake"
			style={`
        left:${flake.left}vw;
        animation-duration:${flake.duration}s;
        font-size:${flake.size}px;
        opacity:${flake.opacity};
      `}
			onanimationend={() => removeFlake(flake.id)}
		>
			<span
				class="snowflake-inner"
				style={`
          animation-duration:${flake.spinDuration}s;
          animation-direction:${flake.spinDirection};
          filter:blur(${flake.blur}px);
        `}
			>
				❄
			</span>
		</div>
	{/each}
</div>

<style lang="sass">
.snow
  position: absolute
  top: -1vh
  left: 0
  width: 100%
  height: 100vh
  pointer-events: none
  overflow: hidden
  z-index: 180

.snowflake
  position: absolute
  top: -1vh
  animation-name: snowFall
  animation-timing-function: linear
  animation-iteration-count: 1

.snowflake-inner
  display: inline-block
  transform-origin: center
  animation-name: spin
  animation-timing-function: linear
  animation-iteration-count: infinite

@keyframes snowFall
  0%
    transform: translate3d(0, -1vh, 0)
  100%
    transform: translate3d(0, 120vh, 0)

@keyframes spin
  from
    transform: rotate(0deg)
  to
    transform: rotate(360deg)

@media (prefers-reduced-motion: reduce)
  .snowflake
    animation: none
    display: none
  .snowflake-inner
    animation: none
    filter: none
</style>
