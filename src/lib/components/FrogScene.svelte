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
		height: 450px;
		border-radius: 2px;
		overflow: hidden;
		background: transparent;
	}
</style>
