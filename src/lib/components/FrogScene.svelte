<script lang="ts">
	import { Canvas } from '@threlte/core';
	import Scene from './Scene.svelte';
</script>

<div class="canvas-wrapper">
	<Canvas>
		<Scene />
	</Canvas>
</div>

<style>
	.canvas-wrapper {
		width: 100%;
		height: 400px;
		border-radius: 16px;
		overflow: hidden;
		border: 1px solid #222;
		background: #0d0d0d;
	}
</style>
