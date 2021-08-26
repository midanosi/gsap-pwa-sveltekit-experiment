<script context="module" lang="ts">
	export const prerender = true;
</script>

<script lang="ts">
	// import Sun from '$lib/icons/Sun.svelte';
	import gsap from 'gsap';
	import { onMount } from 'svelte';
	// import { useMachine } from '@xstate/svelte';
	// import { createMachine, assign } from 'xstate';
	// import Step1 from '$lib/sceneA/Step1.svelte';

	const setCurrentLabel = () => {
		currentLabel = tl.currentLabel();
		// console.log(`currentLabel`, currentLabel);
	};

	let currentLabel = '';
	let tl = gsap.timeline({
		onUpdate: setCurrentLabel,
		// paused: true
		delay: 0.5
	});

	onMount(() => {
		tl.addLabel('start')
			.from('#intro-text-layer', { autoAlpha: 0 })
			.addLabel('intro')
			.to('#intro-text-layer', { autoAlpha: 0 }, '+=2')
			.from('#outside-house-layer', { autoAlpha: 0 })
			.from('#house', { autoAlpha: 0 })
			.from('#sun', { autoAlpha: 0 })
			.from(
				'#circle',
				{
					'stroke-dashoffset': 2 * Math.PI * 4
				},
				'<'
			)
			.from('#sun', {
				rotation: -40,
				ease: 'ease.in'
			})
			.from(
				'#outer-dashes',
				{
					ease: 'ease.in',
					opacity: 0
				},
				'-=0.4'
			)
			.addLabel('house');
		tl.play();
		// tl.play('house');
	});
</script>

<section id="gsap-debugger">
	<span>gsap current label</span>
	<h1>{currentLabel}</h1>
</section>
<section id="animation-surface">
	<div id="intro-text-layer">
		<div class="textbox">
			<h3>Introduction to Scene A</h3>
			<p>Common problem is being outside house and not knowing what's inside</p>
		</div>
	</div>
	<div id="outside-house-layer">
		<div id="svgs-container">
			<svg
				xmlns="http://www.w3.org/2000/svg"
				id="house"
				width="200px"
				height="200px"
				viewBox="0 0 24 24"
				stroke-width="1.5"
				stroke="black"
				fill="brown"
				stroke-linecap="round"
				stroke-linejoin="round"
			>
				<path stroke="none" d="M0 0h24v24H0z" fill="none" />
				<polyline points="5 12 3 12 12 3 21 12 19 12" />
				<path d="M5 12v7a2 2 0 0 0 2 2h10a2 2 0 0 0 2 -2v-7" />
				<path d="M9 21v-6a2 2 0 0 1 2 -2h2a2 2 0 0 1 2 2v6" />
			</svg>
			<svg
				id="sun"
				xmlns="http://www.w3.org/2000/svg"
				viewBox="0 0 24 24"
				stroke-width="2"
				stroke="currentColor"
				fill="none"
				stroke-linecap="round"
				stroke-linejoin="round"
				width="4rem"
				height="4rem"
			>
				<circle id="circle" cx="12" cy="12" r="4" stroke-dasharray={2 * Math.PI * 4} fill="gold" />
				<path
					id="outer-dashes"
					d="M3 12h1m8 -9v1m8 8h1m-9 8v1m-6.4 -15.4l.7 .7m12.1 -.7l-.7 .7m0 11.4l.7 .7m-12.1 -.7l-.7 .7"
				/>
			</svg>
		</div>
	</div>
</section>

<svelte:head>
	<title>Scene A</title>
</svelte:head>

<style>
	section {
		display: flex;
		flex-direction: column;
		align-items: center;
		flex: 1;
	}
	.textbox {
		padding: 1rem;
		border: 3px solid maroon;
		border-radius: 20px;
	}

	#animation-surface {
		position: relative;
		width: 100%;
	}
	#animation-surface > div {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}
	#gsap-debugger {
		position: absolute;
		top: 0;
		left: 0;
	}

	#svgs-container {
		position: relative;
		width: 600px;
		height: 400px;
	}
	#house {
		position: absolute;
		bottom: 0;
		left: 50%;
		transform: translateX(-50%);
	}
	#sun {
		position: absolute;
		top: 0;
		right: 20%;
	}
</style>
