<script lang="ts">
	import { T, useTask } from '@threlte/core';
	import { Float, OrbitControls } from '@threlte/extras';
	import * as THREE from 'three';

	let groupRef: THREE.Group;
	let time = 0;
	let hovered = false;

	useTask((delta) => {
		time += delta;
		if (groupRef) {
			groupRef.rotation.y += delta * 0.3;
			groupRef.position.y = Math.sin(time * 0.8) * 0.05;
		}
	});

	const green = '#22c55e';
	const darkGreen = '#16a34a';
	const glow = '#4ade80';
	const dark = '#111';
</script>

<!-- Lights -->
<T.AmbientLight intensity={0.3} />
<T.PointLight position={[3, 4, 5]} intensity={0.8} color={glow} />
<T.PointLight position={[-4, 2, -3]} intensity={0.4} color="#0ff" />
<T.PointLight position={[0, -3, 2]} intensity={0.2} color="#f0f" />

<!-- Camera -->
<T.PerspectiveCamera makeDefault position={[0, 0.5, 4.5]} fov={40}>
	<OrbitControls
		enableZoom={false}
		enablePan={false}
		autoRotate
		autoRotateSpeed={0.5}
		minPolarAngle={Math.PI / 3}
		maxPolarAngle={Math.PI / 1.7}
	/>
</T.PerspectiveCamera>

<Float floatIntensity={1} speed={1.5}>
	<T.Group bind:ref={groupRef}>
		<!-- Body — wireframe sphere -->
		<T.Mesh
			on:pointerenter={() => hovered = true}
			on:pointerleave={() => hovered = false}
		>
			<T.IcosahedronGeometry args={[1, 1]} />
			<T.MeshStandardMaterial
				color={hovered ? glow : green}
				wireframe
				transparent
				opacity={0.7}
			/>
		</T.Mesh>

		<!-- Inner core -->
		<T.Mesh scale={0.4}>
			<T.OctahedronGeometry args={[1, 0]} />
			<T.MeshStandardMaterial
				color={glow}
				emissive={glow}
				emissiveIntensity={0.5}
				transparent
				opacity={0.6}
			/>
		</T.Mesh>

		<!-- Eye Left -->
		<T.Mesh position={[-0.35, 0.4, 0.75]}>
			<T.SphereGeometry args={[0.12, 16, 16]} />
			<T.MeshStandardMaterial
				color="#fff"
				emissive="#fff"
				emissiveIntensity={0.3}
			/>
		</T.Mesh>
		<T.Mesh position={[-0.35, 0.4, 0.88]}>
			<T.SphereGeometry args={[0.06, 8, 8]} />
			<T.MeshStandardMaterial color={dark} />
		</T.Mesh>

		<!-- Eye Right -->
		<T.Mesh position={[0.35, 0.4, 0.75]}>
			<T.SphereGeometry args={[0.12, 16, 16]} />
			<T.MeshStandardMaterial
				color="#fff"
				emissive="#fff"
				emissiveIntensity={0.3}
			/>
		</T.Mesh>
		<T.Mesh position={[0.35, 0.4, 0.88]}>
			<T.SphereGeometry args={[0.06, 8, 8]} />
			<T.MeshStandardMaterial color={dark} />
		</T.Mesh>

		<!-- Floating ring -->
		<T.Mesh rotation={[Math.PI / 2, 0, 0]} position={[0, 0, 0]}>
			<T.TorusGeometry args={[1.5, 0.015, 16, 64]} />
			<T.MeshStandardMaterial
				color={glow}
				emissive={glow}
				emissiveIntensity={0.8}
				transparent
				opacity={0.3}
			/>
		</T.Mesh>

		<!-- Scattered particles -->
		{#each Array(20) as _, i}
			{@const angle = (i / 20) * Math.PI * 2}
			{@const radius = 1.8 + Math.sin(i * 1.7) * 0.4}
			{@const y = Math.cos(i * 2.3) * 0.8}
			<T.Mesh position={[Math.cos(angle) * radius, y, Math.sin(angle) * radius]}>
				<T.BoxGeometry args={[0.03, 0.03, 0.03]} />
				<T.MeshStandardMaterial
					color={glow}
					emissive={glow}
					emissiveIntensity={1}
					transparent
					opacity={0.5}
				/>
			</T.Mesh>
		{/each}
	</T.Group>
</Float>
