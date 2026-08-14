<script lang="ts">
	import { onMount } from 'svelte';

	let canvas: HTMLCanvasElement;
	let pointerX = 0;
	let pointerY = 0;
	let hasPosition = false;

	interface Line {
		x1: number;
		y1: number;
		x2: number;
		y2: number;
		bornAt: number;
	}
	let lines: Line[] = [];

	const LIFETIME_MS = 150; // each line fully fades within this window
	const SPAWN_INTERVAL_MS = 100; // how often a new burst fires
	const MIN_PER_BURST = 5;
	const MAX_PER_BURST = 10;
	const SAFETY_CAP = 10; // hard cap on number of lines drawn

	function randomEdgePoint(width: number, height: number) {
		const edge = Math.floor(Math.random() * 4);
		switch (edge) {
			case 0: return { x: Math.random() * width, y: 0 };
			case 1: return { x: width, y: Math.random() * height };
			case 2: return { x: Math.random() * width, y: height };
			default: return { x: 0, y: Math.random() * height };
		}
	}

	onMount(() => {
		const ctx = canvas.getContext('2d', { alpha: true });
		if (!ctx) return;

		let dpr = Math.min(window.devicePixelRatio || 1, 2);

		const resize = () => {
			canvas.width = window.innerWidth * dpr;
			canvas.height = window.innerHeight * dpr;
			ctx.setTransform(dpr, 0, 0, dpr, 0, 0);
		};
		let resizeTimeout: number;
		const handleResize = () => {
			clearTimeout(resizeTimeout);
			resizeTimeout = window.setTimeout(resize, 150);
		};
		window.addEventListener('resize', handleResize);
		resize();

		const handleMouseMove = (e: MouseEvent) => {
			pointerX = e.clientX;
			pointerY = e.clientY;
			hasPosition = true;
		};
		window.addEventListener('mousemove', handleMouseMove, { passive: true });

		const handleTouch = (e: TouchEvent) => {
			const touch = e.touches[0] ?? e.changedTouches[0];
			if (touch) {
				pointerX = touch.clientX;
				pointerY = touch.clientY;
				hasPosition = true;
			}
		};
		window.addEventListener('touchstart', handleTouch, { passive: true });
		window.addEventListener('touchmove', handleTouch, { passive: true });

		const spawnBurst = (width: number, height: number, now: number) => {
			if (!hasPosition) return;
			const count = MIN_PER_BURST + Math.floor(Math.random() * (MAX_PER_BURST - MIN_PER_BURST + 1));
			for (let i = 0; i < count; i++) {
				if (lines.length >= SAFETY_CAP) break;
				const { x, y } = randomEdgePoint(width, height);
				const MIN_OFFSET = 10;
				const MAX_OFFSET = 50;

				const dx = x - pointerX;
				const dy = y - pointerY;
				const distance = Math.hypot(dx, dy);

				const offsetDistance =
					MIN_OFFSET + Math.random() * (MAX_OFFSET - MIN_OFFSET);

				const offsetX = (dx / distance) * offsetDistance;
				const offsetY = (dy / distance) * offsetDistance;

				lines.push({
					x1: x,
					y1: y,
					x2: pointerX + offsetX,
					y2: pointerY + offsetY,
					bornAt: now
				});
			}
		};

		const TARGET_FPS = 30;
		const FRAME_MS = 1000 / TARGET_FPS;
		let lastFrameTime = 0;
		let spawnAccumulator = 0;
		let rafId: number;

		const draw = (now: number) => {
			rafId = requestAnimationFrame(draw);
			if (now - lastFrameTime < FRAME_MS) return;
			const dt = now - lastFrameTime;
			lastFrameTime = now;

			const width = window.innerWidth;
			const height = window.innerHeight;

			spawnAccumulator += dt;
			while (spawnAccumulator >= SPAWN_INTERVAL_MS) {
				spawnBurst(width, height, now);
				spawnAccumulator -= SPAWN_INTERVAL_MS;
			}

			ctx.clearRect(0, 0, width, height);
			ctx.strokeStyle = 'rgba(255, 255, 255, 0.2)';
			ctx.lineWidth = 1;

			let writeIndex = 0;
			for (let i = 0; i < lines.length; i++) {
				const line = lines[i];
				const age = now - line.bornAt;
				if (age >= LIFETIME_MS) continue; // dead, drop it

				const opacity = (1 - age / LIFETIME_MS) * 0.5;
				ctx.globalAlpha = opacity;
				ctx.beginPath();
				ctx.moveTo(line.x1, line.y1);
				ctx.lineTo(line.x2, line.y2); // frozen at spawn, this line never moves again
				ctx.stroke();

				lines[writeIndex++] = line;
			}
			lines.length = writeIndex;
		};
		rafId = requestAnimationFrame(draw);

		return () => {
			cancelAnimationFrame(rafId);
			clearTimeout(resizeTimeout);
			window.removeEventListener('resize', handleResize);
			window.removeEventListener('mousemove', handleMouseMove);
			window.removeEventListener('touchstart', handleTouch);
			window.removeEventListener('touchmove', handleTouch);
		};
	});
</script>

<canvas bind:this={canvas} class="bg-canvas"></canvas>

<style>
	.bg-canvas {
		position: fixed;
		top: 0;
		left: 0;
		width: 100vw;
		height: 100vh;
		z-index: -1;
		pointer-events: none;
		background-color: #262626;
	}
</style>