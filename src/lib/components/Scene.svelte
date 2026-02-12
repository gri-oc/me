<script lang="ts">
	import { T, useTask } from '@threlte/core';
	import { Float, OrbitControls } from '@threlte/extras';
	import * as THREE from 'three';

	let groupRef: THREE.Group;
	let eyeL: THREE.Mesh;
	let eyeR: THREE.Mesh;
	let pupilL: THREE.Mesh;
	let pupilR: THREE.Mesh;
	let mouthRef: THREE.Mesh;

	let hovered = false;
	let time = 0;

	useTask((delta) => {
		time += delta;
		if (groupRef) {
			groupRef.rotation.y = Math.sin(time * 0.5) * 0.15;
		}
	});

	const bodyColor = '#22c55e';
	const bellyColor = '#86efac';
	const eyeWhite = '#f0fdf4';
	const pupilColor = '#0a0a0a';
	const mouthColor = '#16a34a';
</script>

<!-- Lights -->
<T.AmbientLight intensity={0.6} />
<T.DirectionalLight position={[5, 5, 5]} intensity={1} />
<T.PointLight position={[-3, 2, 4]} intensity={0.5} color="#4ade80" />

<!-- Camera -->
<T.PerspectiveCamera makeDefault position={[0, 1, 5]} fov={45}>
	<OrbitControls
		enableZoom={false}
		enablePan={false}
		autoRotate
		autoRotateSpeed={1}
		minPolarAngle={Math.PI / 3}
		maxPolarAngle={Math.PI / 1.8}
	/>
</T.PerspectiveCamera>

<!-- Frog -->
<Float floatIntensity={2} speed={2}>
	<T.Group bind:ref={groupRef}>
		<!-- Body -->
		<T.Mesh position={[0, 0, 0]} scale={[1.3, 1, 1.1]}
			on:pointerenter={() => hovered = true}
			on:pointerleave={() => hovered = false}
		>
			<T.SphereGeometry args={[1, 32, 32]} />
			<T.MeshStandardMaterial color={hovered ? '#4ade80' : bodyColor} />
		</T.Mesh>

		<!-- Belly -->
		<T.Mesh position={[0, -0.1, 0.55]} scale={[0.9, 0.75, 0.5]}>
			<T.SphereGeometry args={[1, 32, 32]} />
			<T.MeshStandardMaterial color={bellyColor} />
		</T.Mesh>

		<!-- Eye Left -->
		<T.Mesh bind:ref={eyeL} position={[-0.45, 0.75, 0.5]} scale={[0.4, 0.45, 0.4]}>
			<T.SphereGeometry args={[1, 32, 32]} />
			<T.MeshStandardMaterial color={eyeWhite} />
		</T.Mesh>
		<T.Mesh bind:ref={pupilL} position={[-0.45, 0.75, 0.85]}>
			<T.SphereGeometry args={[0.18, 16, 16]} />
			<T.MeshStandardMaterial color={pupilColor} />
		</T.Mesh>

		<!-- Eye Right -->
		<T.Mesh bind:ref={eyeR} position={[0.45, 0.75, 0.5]} scale={[0.4, 0.45, 0.4]}>
			<T.SphereGeometry args={[1, 32, 32]} />
			<T.MeshStandardMaterial color={eyeWhite} />
		</T.Mesh>
		<T.Mesh bind:ref={pupilR} position={[0.45, 0.75, 0.85]}>
			<T.SphereGeometry args={[0.18, 16, 16]} />
			<T.MeshStandardMaterial color={pupilColor} />
		</T.Mesh>

		<!-- Mouth -->
		<T.Mesh bind:ref={mouthRef} position={[0, -0.2, 0.95]} rotation={[0.2, 0, 0]} scale={[0.5, 0.12, 0.1]}>
			<T.BoxGeometry args={[1, 1, 1]} />
			<T.MeshStandardMaterial color={mouthColor} />
		</T.Mesh>

		<!-- Left Foot -->
		<T.Mesh position={[-0.6, -1, 0.3]} rotation={[-0.3, 0.3, 0]} scale={[0.4, 0.12, 0.55]}>
			<T.BoxGeometry args={[1, 1, 1]} />
			<T.MeshStandardMaterial color={bodyColor} />
		</T.Mesh>

		<!-- Right Foot -->
		<T.Mesh position={[0.6, -1, 0.3]} rotation={[-0.3, -0.3, 0]} scale={[0.4, 0.12, 0.55]}>
			<T.BoxGeometry args={[1, 1, 1]} />
			<T.MeshStandardMaterial color={bodyColor} />
		</T.Mesh>
	</T.Group>
</Float>
