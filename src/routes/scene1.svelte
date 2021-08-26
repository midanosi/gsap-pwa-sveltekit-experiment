<script context="module" lang="ts">
	export const prerender = true;
</script>

<script lang="ts">
	import Sun from '$lib/icons/Sun.svelte';
	import gsap from 'gsap';
	import { onMount } from 'svelte';

	let visible = false;
	const duration = 1;

	function tweenIn(node, somethingelse) {
		console.log(`somethingelse`, somethingelse);
		let tl = gsap.timeline();

		tl.from(node, {
			duration,
			opacity: 0
		})
			.from(
				'#circle',
				{
					duration,
					'stroke-dashoffset': 2 * Math.PI * 4
					// 'stroke-dasharray': 2 * Math.PI * 4
				},
				`-=${duration * 0.75}`
			)
			.from(
				'.icon',
				{
					duration,
					rotation: -40,
					ease: 'ease.in'
				},
				`-=${duration * 0.1}`
			)
			.from(
				'#outer-dashes',
				{
					ease: 'ease.in',
					opacity: 0
				},
				`-=${duration * 0.8}`
			);

		return {
			/* GSAP's duration is in seconds. Svelte's is in miliseconds */
			duration: duration * 1000,
			tick: (t) => {
				tl.progress(t);
			}
		};
	}
</script>

<section>
	<h1>Scene 1</h1>
	<label>
		visible
		<input bind:checked={visible} type="checkbox" />
	</label>
	{#if visible}
		<svg
			xmlns="http://www.w3.org/2000/svg"
			class="icon icon-tabler icon-tabler-sun"
			id="icon-tabler-sun"
			viewBox="0 0 24 24"
			stroke-width="2"
			stroke="currentColor"
			fill="none"
			stroke-linecap="round"
			stroke-linejoin="round"
			width="4rem"
			height="4rem"
			in:tweenIn
		>
			<circle id="circle" cx="12" cy="12" r="4" stroke-dasharray={2 * Math.PI * 4} fill="gold" />
			<path
				id="outer-dashes"
				d="M3 12h1m8 -9v1m8 8h1m-9 8v1m-6.4 -15.4l.7 .7m12.1 -.7l-.7 .7m0 11.4l.7 .7m-12.1 -.7l-.7 .7"
			/>
		</svg>
	{/if}
</section>

<svelte:head>
	<title>Scene1</title>
</svelte:head>

<style>
	section {
		display: flex;
		flex-direction: column;
		align-items: center;
		flex: 1;
	}
</style>
