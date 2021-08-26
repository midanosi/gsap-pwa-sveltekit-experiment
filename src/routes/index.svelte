<script context="module" lang="ts">
	export const prerender = true;
</script>

<script lang="ts">
	import Sun from '$lib/icons/Sun.svelte';
	import gsap from 'gsap';
	import { onMount } from 'svelte';

	const duration = 1;

	onMount(() => {
		let tl = gsap.timeline();
		tl.from('#welcome-text', {
			autoAlpha: 0,
			duration: 0.5,
			y: 20,
			ease: 'power1'
		}).from('.scene-button', { opacity: 0, y: 10, stagger: 0.1 }, '+=0.3');

		return {
			// duration: duration * 1000,
			tick: (t) => tl.progress(t)
		};
	});
</script>

<section>
	<h1 id="welcome-text">Welcome to this demo</h1>
	<div class="scene-menu">
		<a href="/scene1" id="scene-1-button" class="scene-button">Scene1</a>
		<a href="/scene2" id="scene-2-button" class="scene-button">Scene2</a>
		<a href="/scene3" id="scene-3-button" class="scene-button">Scene3</a>
	</div>
</section>

<svelte:head>
	<title>Home</title>
</svelte:head>

<style>
	section {
		display: flex;
		flex-direction: column;
		align-items: center;
		flex: 1;
		gap: 1rem;
	}
	.scene-menu {
		display: flex;
		gap: 1rem;
	}
	.scene-button {
		padding: 2rem;
		border: 1px solid grey;
		border-radius: 15px;
		font-size: 1.4rem;
	}
	.scene-button:hover {
		background: #eee;
		transition: background 0.3s;
	}
</style>
