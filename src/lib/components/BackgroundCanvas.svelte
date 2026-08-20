<script lang="ts">
	import { onMount } from "svelte";

	let canvas: HTMLCanvasElement;

	const CONFIG = {
		targetFps: 60,
		lifetimeFrames: 5, // line fades out over this many "frames" worth of time
		burstMin: 2,
		burstMax: 4,
		maxLines: 7, // hard cap on simultaneous lines
		endpointOffsetMin: 10, // how far the line's tip sits from the pointer, min/max px
		endpointOffsetMax: 50,
		strokeStyle: "rgba(255, 255, 255, 0.2)",
		lineWidth: 1,
		maxOpacity: 0.5,
		resizeDebounceMs: 150,
		maxDpr: 2
	};
	const SPAWN_INTERVAL_MS = 1000 / CONFIG.targetFps;
	const LIFETIME_MS = SPAWN_INTERVAL_MS * CONFIG.lifetimeFrames;

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

	function randomEdgePoint(width: number, height: number) {
		const edge = Math.floor(Math.random() * 4);
		switch (edge) {
			case 0: return { x: Math.random() * width, y: 0 };
			case 1: return { x: width, y: Math.random() * height };
			case 2: return { x: Math.random() * width, y: height };
			default: return { x: 0, y: Math.random() * height };
		}
	}

	function computeEndpoint(originX: number, originY: number) {
		const dx = originX - pointerX;
		const dy = originY - pointerY;
		const distance = Math.hypot(dx, dy);

		const offsetDistance =
			CONFIG.endpointOffsetMin +
			Math.random() * (CONFIG.endpointOffsetMax - CONFIG.endpointOffsetMin);

		return {
			x: pointerX + (dx / distance) * offsetDistance,
			y: pointerY + (dy / distance) * offsetDistance
		};
	}

	function createLine(width: number, height: number, now: number): Line {
		const origin = randomEdgePoint(width, height);
		const endpoint = computeEndpoint(origin.x, origin.y);
		return { x1: origin.x, y1: origin.y, x2: endpoint.x, y2: endpoint.y, bornAt: now };
	}

	function spawnBurst(width: number, height: number, now: number) {
		if (!hasPosition) return;
		const count = CONFIG.burstMin + Math.floor(Math.random() * (CONFIG.burstMax - CONFIG.burstMin + 1));
		for (let i = 0; i < count; i++) {
			if (lines.length >= CONFIG.maxLines) break;
			lines.push(createLine(width, height, now));
		}
	}

	function opacityForAge(age: number) {
		return (1 - age / LIFETIME_MS) * CONFIG.maxOpacity;
	}

	function drawLine(ctx: CanvasRenderingContext2D, line: Line, age: number) {
		ctx.globalAlpha = opacityForAge(age);
		ctx.beginPath();
		ctx.moveTo(line.x1, line.y1);
		ctx.lineTo(line.x2, line.y2);
		ctx.stroke();
	}
	
	function updateAndDrawLines(ctx: CanvasRenderingContext2D, now: number) {
		let writeIndex = 0;
		for (let i = 0; i < lines.length; i++) {
			const line = lines[i];
			const age = now - line.bornAt;
			if (age >= LIFETIME_MS) continue; // dead, drop it

			drawLine(ctx, line, age);
			lines[writeIndex++] = line;
		}
		lines.length = writeIndex;
	}

	function trackMouse(e: MouseEvent) {
		pointerX = e.clientX;
		pointerY = e.clientY;
		hasPosition = true;
	}

	function trackTouch(e: TouchEvent) {
		const touch = e.touches[0] ?? e.changedTouches[0];
		if (touch) {
			pointerX = touch.clientX;
			pointerY = touch.clientY;
			hasPosition = true;
		}
	}
	
	function makeResizeHandler(canvas: HTMLCanvasElement, ctx: CanvasRenderingContext2D, dpr: number) {
		const resize = () => {
			canvas.width = window.innerWidth * dpr;
			canvas.height = window.innerHeight * dpr;
			ctx.setTransform(dpr, 0, 0, dpr, 0, 0);
		};
		let resizeTimeout: number;
		const debouncedResize = () => {
			clearTimeout(resizeTimeout);
			resizeTimeout = window.setTimeout(resize, CONFIG.resizeDebounceMs);
		};
		return { resize, debouncedResize, clearPending: () => clearTimeout(resizeTimeout) };
	}

	function makeDrawLoop(ctx: CanvasRenderingContext2D) {
		let lastFrameTime = 0;
		let spawnAccumulator = 0;
		let rafId: number;

		const frame = (now: number) => {
			rafId = requestAnimationFrame(frame);
			if (now - lastFrameTime < SPAWN_INTERVAL_MS) return;
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
			ctx.strokeStyle = CONFIG.strokeStyle;
			ctx.lineWidth = CONFIG.lineWidth;
			updateAndDrawLines(ctx, now);
		};

		return {
			start: () => (rafId = requestAnimationFrame(frame)),
			stop: () => cancelAnimationFrame(rafId)
		};
	}

	onMount(() => {
		const ctx = canvas.getContext("2d", { alpha: true });
		if (!ctx) return;

		const dpr = Math.min(window.devicePixelRatio || 1, CONFIG.maxDpr);
		const { resize, debouncedResize, clearPending } = makeResizeHandler(canvas, ctx, dpr);
		resize();

		window.addEventListener("resize", debouncedResize);
		window.addEventListener("mousemove", trackMouse, { passive: true });
		window.addEventListener("touchstart", trackTouch, { passive: true });
		window.addEventListener("touchmove", trackTouch, { passive: true });

		const loop = makeDrawLoop(ctx);
		loop.start();

		return () => {
			loop.stop();
			clearPending();
			window.removeEventListener("resize", debouncedResize);
			window.removeEventListener("mousemove", trackMouse);
			window.removeEventListener("touchstart", trackTouch);
			window.removeEventListener("touchmove", trackTouch);
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
		background-color: var(--global-bg);
	}
</style>