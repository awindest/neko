<script lang="ts">
	import { mousePos } from '$lib/runes/runes.svelte.js' // mouse pos is a rune that captures the coordinates of the pointer when the pointer moves

	let canvas: HTMLCanvasElement = $state()
	let context: CanvasRenderingContext2D = $state()

	let w = $state()
	let h = $state()

	let x1 = $state()
	let y1 = $state()
	let x2 = $state()
	let y2 = $state()

	$effect(() => {
		context = canvas.getContext('2d')

		function resize() {
			canvas.width = window.innerWidth
			canvas.height = window.innerHeight
			x1 = window.innerWidth / 2
			y1 = window.innerHeight
		}

		window.addEventListener('resize', resize)
		resize()

		drawLaserBeam()

		return () => {
			window.removeEventListener('resize', resize)
		}
	})

	function drawLaserBeam() {
		x2 = mousePos.X
		y2 = mousePos.Y

		context.clearRect(0, 0, canvas.width, canvas.height)

		context.strokeStyle = 'red'
		context.lineWidth = 1.5
		context.shadowColor = 'red'
		context.shadowBlur = 8
		context.beginPath()

		context.moveTo(x1, y1)
		context.lineTo(x2, y2)

		context.stroke()
	}
</script>

<div
	class="pointer-container"
	bind:clientWidth={w}
	bind:clientHeight={h}
	style={`left: ${mousePos.X - 8}px; top: ${mousePos.Y - 8}px;`}
>
	<div class="pointer" aria-hidden="true"></div>
</div>
<canvas bind:this={canvas}></canvas>

<style>
	.pointer-container {
		width: 100%;
		height: 100%;
		position: fixed;
		top: 0;
		left: 0;
		border: red;
	}

	.pointer {
		width: 32px;
		height: 32px;
		background-image: url('$lib/images/laserPointer.webp');
		background-position: center;
		background-repeat: none;
		background-size: cover;
		z-index: -5000;
	}
</style>
