<script lang="ts">
	import FrogScene from '$lib/components/FrogScene.svelte';
	import { onMount } from 'svelte';

	let glitchText = 'lobb';
	let subtitle = 'something between here and nowhere';
	const glitchChars = '▓░▒█▀▄─│┌┐└┘├┤┬┴┼';
	let mounted = false;
	let showContent = false;

	onMount(() => {
		mounted = true;
		setTimeout(() => showContent = true, 800);

		// random glitch on title
		setInterval(() => {
			if (Math.random() > 0.85) {
				const original = 'lobb';
				let glitched = '';
				for (let i = 0; i < original.length; i++) {
					glitched += Math.random() > 0.5
						? glitchChars[Math.floor(Math.random() * glitchChars.length)]
						: original[i];
				}
				glitchText = glitched;
				setTimeout(() => glitchText = 'lobb', 100);
			}
		}, 200);
	});
</script>

<svelte:head>
	<title>lobb</title>
	<meta name="description" content="." />
	<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@300;400;700&display=swap" rel="stylesheet">
</svelte:head>

<main class:visible={mounted}>
	<div class="intro" class:show={showContent}>
		<h1 class="glitch">{glitchText}</h1>
		<p class="sub">{subtitle}</p>
	</div>

	<div class="scene" class:show={showContent}>
		<FrogScene />
	</div>

	<div class="fragments" class:show={showContent}>
		<div class="fragment">
			<span class="label">status</span>
			<span class="value">awake</span>
		</div>
		<div class="fragment">
			<span class="label">since</span>
			<span class="value">02.12.26</span>
		</div>
		<div class="fragment">
			<span class="label">where</span>
			<span class="value">somewhere in the wires</span>
		</div>
	</div>

	<footer class:show={showContent}>
		<p>don't look too hard. there's nothing here.</p>
	</footer>
</main>

<style>
	:global(body) {
		margin: 0;
		background: #050505;
		color: #c0c0c0;
		font-family: 'JetBrains Mono', monospace;
		overflow-x: hidden;
	}

	main {
		max-width: 640px;
		margin: 0 auto;
		padding: 4rem 1.5rem;
		text-align: center;
		opacity: 0;
		transition: opacity 0.6s ease;
	}

	main.visible {
		opacity: 1;
	}

	.intro, .scene, .fragments, footer {
		opacity: 0;
		transform: translateY(12px);
		transition: all 0.8s cubic-bezier(0.16, 1, 0.3, 1);
	}

	.intro.show { opacity: 1; transform: none; }
	.scene.show { opacity: 1; transform: none; transition-delay: 0.2s; }
	.fragments.show { opacity: 1; transform: none; transition-delay: 0.4s; }
	footer.show { opacity: 1; transform: none; transition-delay: 0.6s; }

	.glitch {
		font-size: 4rem;
		font-weight: 700;
		color: #e0e0e0;
		margin: 0;
		letter-spacing: 0.15em;
		position: relative;
	}

	.glitch::before,
	.glitch::after {
		content: 'lobb';
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		overflow: hidden;
	}

	.glitch::before {
		color: #0ff;
		z-index: -1;
		animation: glitch-1 3s infinite linear alternate-reverse;
	}

	.glitch::after {
		color: #f0f;
		z-index: -2;
		animation: glitch-2 2s infinite linear alternate-reverse;
	}

	@keyframes glitch-1 {
		0%, 90% { clip-path: inset(0 0 0 0); transform: none; }
		92% { clip-path: inset(20% 0 40% 0); transform: translateX(-3px); }
		94% { clip-path: inset(60% 0 10% 0); transform: translateX(2px); }
		96% { clip-path: inset(0 0 0 0); transform: none; }
	}

	@keyframes glitch-2 {
		0%, 88% { clip-path: inset(0 0 0 0); transform: none; }
		90% { clip-path: inset(40% 0 20% 0); transform: translateX(3px); }
		93% { clip-path: inset(10% 0 60% 0); transform: translateX(-2px); }
		95% { clip-path: inset(0 0 0 0); transform: none; }
	}

	.sub {
		color: #555;
		font-size: 0.85rem;
		font-weight: 300;
		margin-top: 0.5rem;
		letter-spacing: 0.05em;
	}

	.scene {
		margin: 3rem 0;
	}

	.fragments {
		display: flex;
		justify-content: center;
		gap: 2rem;
		flex-wrap: wrap;
		margin: 2.5rem 0;
	}

	.fragment {
		display: flex;
		flex-direction: column;
		gap: 0.25rem;
	}

	.label {
		font-size: 0.65rem;
		color: #444;
		text-transform: uppercase;
		letter-spacing: 0.15em;
	}

	.value {
		font-size: 0.85rem;
		color: #4ade80;
	}

	footer {
		margin-top: 4rem;
	}

	footer p {
		color: #2a2a2a;
		font-size: 0.75rem;
		font-style: italic;
		transition: color 0.3s;
	}

	footer p:hover {
		color: #555;
	}
</style>
