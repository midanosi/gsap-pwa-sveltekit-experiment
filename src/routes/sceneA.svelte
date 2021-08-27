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
		// INTRO
		tl.addLabel('start').to('#intro-text-layer', { autoAlpha: 1 }).addLabel('intro');

		// SHOW HOUSE
		tl.to('#intro-text-layer', { autoAlpha: 0 }, '+=2')
			.to('#outside-house-layer', { autoAlpha: 1 })
			.to('#house', { autoAlpha: 1 })
			.to('#sun', { autoAlpha: 1 })
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

		// PERSON + TEXT
		tl.to('#house', { left: '30%' })
			.to('#sun', { right: '15%' }, '<')
			.to('#person', { autoAlpha: 1 }, '+=0.5')
			.to('#text1', { autoAlpha: 1 }, '+=1')
			.to('#text2', { autoAlpha: 1 }, '+=2')
			.addLabel('personoutside');

		// PLANE + ARROW + MORE TEXT
		tl.to(['#text1', '#text2'], { autoAlpha: 0 }, '+=1')
			.to(['#plane', '#moon'], { autoAlpha: 1 }, '+=1')
			.to('#sun', { autoAlpha: 0 }, '<')
			.to('#dashes', { autoAlpha: 1 }, '+=0.5')
			.to('#text3', { autoAlpha: 1 }, '+=1')
			.addLabel('planeoutside');

		// NEW SCENE WITH FLOORPLAN
		tl.to(['#house', '#plane', '#sun', '#moon', '#dashes', '#text3', '#person'], { autoAlpha: 0 });
		tl.addLabel('picture-start');

		tl.to('#person', { left: '100px' }, '+=0.8')
			.to('#person', { autoAlpha: 1, left: '20px', width: '150px', height: '150px' }, '+=0.2')
			.to('#picture-border', { autoAlpha: 1 })
			.to(['#room-chooser', '#floorplan'], { autoAlpha: 1 }, '+=1')
			.to(['#bookcase', '#window'], { autoAlpha: 1 }, '+=1')
			.to(['#window-ai-highlight', '#bookcase-ai-highlight'], { autoAlpha: 1 }, '+=1');

		tl.addLabel('picture-end');

		tl.play();
		// tl.play('picture-start');
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
			<svg
				id="person"
				enable-background="new 0 0 505.136 505.136"
				height="512"
				viewBox="0 0 505.136 505.136"
				width="512"
				xmlns="http://www.w3.org/2000/svg"
				><g
					><path
						d="m312.029 123.191h-118.943c-22.299.027-40.37 18.098-40.397 40.397v146.748c0 7.859 6.371 14.229 14.229 14.229 7.859 0 14.229-6.371 14.229-14.229v-120.182c0-4.365 3.539-7.904 7.904-7.904s7.904 3.539 7.904 7.904v287.583c.455 13.163 11.495 23.465 24.658 23.011 12.525-.433 22.578-10.486 23.011-23.011v-164.242c0-4.365 3.539-7.904 7.904-7.904s7.904 3.539 7.904 7.904v164.243c.455 13.163 11.495 23.465 24.658 23.011 12.525-.433 22.578-10.486 23.011-23.011v-287.584c0-4.365 3.539-7.904 7.904-7.904s7.904 3.539 7.904 7.904v120.181c0 7.859 6.371 14.229 14.229 14.229 7.859 0 14.229-6.371 14.229-14.229v-146.74c-.021-22.28-18.058-40.345-40.338-40.404z"
					/><path
						d="m284.303 505.136c-15.125-.016-27.383-12.273-27.399-27.399v-164.242c0-2.403-1.948-4.351-4.351-4.351s-4.351 1.948-4.351 4.351v164.243c-.345 15.128-12.888 27.112-28.016 26.767-14.643-.334-26.434-12.125-26.767-26.767v-287.584c0-2.401-1.946-4.347-4.347-4.347s-4.347 1.946-4.347 4.347v120.181c0 9.823-7.963 17.787-17.787 17.787s-17.787-7.963-17.787-17.787v-146.74c.027-24.264 19.691-43.927 43.955-43.955h118.922c24.264.027 43.927 19.691 43.955 43.955v146.741c0 9.823-7.963 17.787-17.787 17.787s-17.787-7.963-17.787-17.787v-120.182c0-2.401-1.946-4.347-4.347-4.347s-4.347 1.946-4.347 4.347v287.583c-.018 15.13-12.282 27.387-27.412 27.399zm-31.746-203.103c6.327.008 11.454 5.135 11.462 11.462v164.243c.297 11.199 9.615 20.036 20.814 19.74 10.781-.286 19.454-8.958 19.74-19.74v-287.584c0-6.33 5.132-11.462 11.462-11.462s11.462 5.132 11.462 11.462v120.181c0 5.894 4.778 10.672 10.672 10.672s10.672-4.778 10.672-10.672v-146.74c-.024-20.336-16.504-36.816-36.84-36.84h-118.915c-20.336.024-36.816 16.504-36.84 36.84v146.741c0 5.894 4.778 10.672 10.672 10.672s10.672-4.778 10.672-10.672v-120.182c0-6.33 5.132-11.462 11.462-11.462s11.462 5.132 11.462 11.462v287.583c.297 11.199 9.615 20.036 20.814 19.74 10.781-.286 19.454-8.958 19.74-19.74v-164.242c.007-6.338 5.151-11.47 11.489-11.462z"
					/><circle cx="252.557" cy="49.902" r="46.352" /><path
						d="m252.557 99.819c-27.564 0-49.91-22.345-49.91-49.91 0-27.564 22.345-49.91 49.91-49.91 27.562 0 49.906 22.341 49.91 49.902-.027 27.556-22.355 49.887-49.91 49.918zm0-92.711c-23.635 0-42.795 19.16-42.795 42.795s19.16 42.795 42.795 42.795 42.795-19.16 42.795-42.795c-.027-23.624-19.171-42.768-42.795-42.795z"
					/></g
				></svg
			>
			<svg
				xmlns="http://www.w3.org/2000/svg"
				id="plane"
				width="44"
				height="44"
				viewBox="0 0 24 24"
				stroke-width="1.5"
				stroke="black"
				fill="none"
				stroke-linecap="round"
				stroke-linejoin="round"
			>
				<path stroke="none" d="M0 0h24v24H0z" fill="none" />
				<path d="M16 10h4a2 2 0 0 1 0 4h-4l-4 7h-3l2 -7h-4l-2 2h-3l2 -4l-2 -4h3l2 2h4l-2 -7h3z" />
			</svg>
			<svg
				xmlns="http://www.w3.org/2000/svg"
				id="dashes"
				width="44"
				height="44"
				viewBox="0 0 24 24"
				stroke-width="1.5"
				stroke="black"
				fill="none"
				stroke-linecap="round"
				stroke-linejoin="round"
			>
				<path d="M1,1 h22 v22" stroke-dasharray="2" />
			</svg>
			<svg
				xmlns="http://www.w3.org/2000/svg"
				id="moon"
				width="44"
				height="44"
				viewBox="0 0 24 24"
				stroke-width="1.5"
				stroke="#ffffff"
				fill="none"
				stroke-linecap="round"
				stroke-linejoin="round"
			>
				<path stroke="none" d="M0 0h24v24H0z" fill="none" />
				<path d="M12 3c.132 0 .263 0 .393 0a7.5 7.5 0 0 0 7.92 12.446a9 9 0 1 1 -8.313 -12.454z" />
			</svg>
			<svg
				version="1.1"
				id="bookcase"
				xmlns="http://www.w3.org/2000/svg"
				xmlns:xlink="http://www.w3.org/1999/xlink"
				x="0px"
				y="0px"
				viewBox="0 0 496 496"
				style="enable-background:new 0 0 496 496;"
				xml:space="preserve"
			>
				<rect
					id="bookcase-ai-highlight"
					x="0"
					y="0"
					width="100%"
					height="100%"
					stroke="red"
					stroke-width="10"
					fill="none"
				/>
				<rect x="88" y="8" style="fill:#896045;" width="320" height="480" />
				<path
					d="M408,496H88c-4.8,0-8-3.2-8-8V8c0-4.8,3.2-8,8-8h320c4.8,0,8,3.2,8,8v480C416,492.8,412.8,496,408,496z M96,480h304V16H96	V480z"
				/>
				<rect x="120" y="40" style="fill:#AF7B58;" width="256" height="96" />
				<path
					d="M376,144H120c-4.8,0-8-3.2-8-8V40c0-4.8,3.2-8,8-8h256c4.8,0,8,3.2,8,8v96C384,140.8,380.8,144,376,144z M128,128h240V48	H128V128z"
				/>
				<rect x="312" y="72" style="fill:#8BB8BD;" width="32" height="64" />
				<path
					d="M344,144h-32c-4.8,0-8-3.2-8-8V72c0-4.8,3.2-8,8-8h32c4.8,0,8,3.2,8,8v64C352,140.8,348.8,144,344,144z M320,128h16V80h-16	V128z"
				/>
				<rect x="344" y="72" style="fill:#8BB8BD;" width="32" height="64" />
				<path
					d="M376,144h-32c-4.8,0-8-3.2-8-8V72c0-4.8,3.2-8,8-8h32c4.8,0,8,3.2,8,8v64C384,140.8,380.8,144,376,144z M352,128h16V80h-16	V128z"
				/>
				<rect x="280" y="72" style="fill:#8BB8BD;" width="32" height="64" />
				<path
					d="M312,144h-32c-4.8,0-8-3.2-8-8V72c0-4.8,3.2-8,8-8h32c4.8,0,8,3.2,8,8v64C320,140.8,316.8,144,312,144z M288,128h16V80h-16	V128z"
				/>
				<rect x="248" y="72" style="fill:#8BB8BD;" width="32" height="64" />
				<path
					d="M280,144h-32c-4.8,0-8-3.2-8-8V72c0-4.8,3.2-8,8-8h32c4.8,0,8,3.2,8,8v64C288,140.8,284.8,144,280,144z M256,128h16V80h-16	V128z"
				/>
				<rect x="216" y="72" style="fill:#8BB8BD;" width="32" height="64" />
				<path
					d="M248,144h-32c-4.8,0-8-3.2-8-8V72c0-4.8,3.2-8,8-8h32c4.8,0,8,3.2,8,8v64C256,140.8,252.8,144,248,144z M224,128h16V80h-16	V128z"
				/>
				<polygon style="fill:#8BB8BD;" points="120,136 184,72 208,96 168,136 " />
				<path
					d="M168,144h-48c-4.8,0-8-3.2-8-8c0-2.4,0.8-4,2.4-5.6l64-64c3.2-3.2,8-3.2,11.2,0l24,24c3.2,3.2,3.2,8,0,11.2l-40,40	C172,143.2,170.4,144,168,144z M139.2,128h25.6l32-32L184,83.2L139.2,128z"
				/>
				<rect x="112" y="160" width="272" height="16" />
				<rect x="120" y="200" style="fill:#AF7B58;" width="256" height="96" />
				<path
					d="M376,304H120c-4.8,0-8-3.2-8-8v-96c0-4.8,3.2-8,8-8h256c4.8,0,8,3.2,8,8v96C384,300.8,380.8,304,376,304z M128,288h240v-80	H128V288z"
				/>
				<rect x="120" y="232" style="fill:#8BB8BD;" width="32" height="64" />
				<path
					d="M152,304h-32c-4.8,0-8-3.2-8-8v-64c0-4.8,3.2-8,8-8h32c4.8,0,8,3.2,8,8v64C160,300.8,156.8,304,152,304z M128,288h16v-48	h-16V288z"
				/>
				<rect x="152" y="232" style="fill:#8BB8BD;" width="32" height="64" />
				<path
					d="M184,304h-32c-4.8,0-8-3.2-8-8v-64c0-4.8,3.2-8,8-8h32c4.8,0,8,3.2,8,8v64C192,300.8,188.8,304,184,304z M160,288h16v-48	h-16V288z"
				/>
				<rect x="184" y="232" style="fill:#8BB8BD;" width="32" height="64" />
				<path
					d="M216,304h-32c-4.8,0-8-3.2-8-8v-64c0-4.8,3.2-8,8-8h32c4.8,0,8,3.2,8,8v64C224,300.8,220.8,304,216,304z M192,288h16v-48	h-16V288z"
				/>
				<rect x="216" y="232" style="fill:#8BB8BD;" width="32" height="64" />
				<path
					d="M248,304h-32c-4.8,0-8-3.2-8-8v-64c0-4.8,3.2-8,8-8h32c4.8,0,8,3.2,8,8v64C256,300.8,252.8,304,248,304z M224,288h16v-48	h-16V288z"
				/>
				<rect x="248" y="232" style="fill:#8BB8BD;" width="32" height="64" />
				<path
					d="M280,304h-32c-4.8,0-8-3.2-8-8v-64c0-4.8,3.2-8,8-8h32c4.8,0,8,3.2,8,8v64C288,300.8,284.8,304,280,304z M256,288h16v-48	h-16V288z"
				/>
				<rect x="120" y="328" style="fill:#AF7B58;" width="256" height="128" />
				<path
					d="M376,464H120c-4.8,0-8-3.2-8-8V328c0-4.8,3.2-8,8-8h256c4.8,0,8,3.2,8,8v128C384,460.8,380.8,464,376,464z M128,448h240V336	H128V448z"
				/>
				<rect x="240" y="328" width="16" height="128" />
				<rect x="120" y="352" width="256" height="16" />
				<rect x="56" y="24" style="fill:#493325;" width="32" height="464" />
				<path
					d="M88,496H56c-4.8,0-8-3.2-8-8V24c0-4.8,3.2-8,8-8h32c4.8,0,8,3.2,8,8v464C96,492.8,92.8,496,88,496z M64,480h16V32H64V480z"
				/>
				<rect x="408" y="24" style="fill:#493325;" width="32" height="464" />
				<path
					d="M440,496h-32c-4.8,0-8-3.2-8-8V24c0-4.8,3.2-8,8-8h32c4.8,0,8,3.2,8,8v464C448,492.8,444.8,496,440,496z M416,480h16V32h-16	V480z"
				/>
				<rect x="208" y="376" width="16" height="32" />
				<rect x="272" y="376" width="16" height="32" />
				<polygon style="fill:#8BB8BD;" points="328,296 288,256 312,232 376,296 " />
				<path
					d="M376,304h-48c-2.4,0-4-0.8-5.6-2.4l-40-40c-3.2-3.2-3.2-8,0-11.2l24-24c3.2-3.2,8-3.2,11.2,0l64,64c3.2,3.2,3.2,8,0,11.2	C380,303.2,378.4,304,376,304z M331.2,288h25.6L312,243.2L299.2,256L331.2,288z"
				/>
			</svg>
			<svg
				xmlns="http://www.w3.org/2000/svg"
				id="window"
				width="44"
				height="44"
				viewBox="0 0 24 24"
				stroke-width="1.5"
				stroke="black"
				fill="none"
				stroke-linecap="round"
				stroke-linejoin="round"
			>
				<rect id="window-rect" x="4" y="4" width="16" height="16" />
				<rect
					id="window-ai-highlight"
					x="0"
					y="0"
					width="24"
					height="24"
					stroke-width="2"
					stroke="green"
				/>
			</svg>
			<svg
				id="floorplan"
				xmlns="http://www.w3.org/2000/svg"
				xmlns:xlink="http://www.w3.org/1999/xlink"
				version="1.1"
				x="0px"
				y="0px"
				viewBox="0 0 100 125"
				enable-background="new 0 0 100 100"
				xml:space="preserve"
			>
				<path
					d="M35.8,5v37.6H25.7v42h17V95h31.7V5H35.8z M32.6,62.9h-5.3V61h5.3V62.9z M32.6,63.5v1.2h-5.3v-1.2H32.6z M32.6,48.2h-5.3  v-1.9h5.3V48.2z M32.6,48.7v1.9h-5.3v-1.9H32.6z M32.6,51.2v1.9h-5.3v-1.9H32.6z M32.6,53.6v1.9h-5.3v-1.9H32.6z M32.6,56.1V58h-5.3  v-1.9H32.6z M32.6,58.6v1.9h-5.3v-1.9H32.6z M32.6,44.2v1.5h-5.3v-1.5H32.6z M44.2,93.4v-9.6V83V66h-1.6v17H27.3V66.3h6.9v-5.5h10  v-4.4h-1.6v2.8h-8.4v-15h3.2V28.7h19.2v-1.6H37.4V6.6h18.4v15.5c-2.7,0.9-3.1,4.1-3.1,4.1l0.6,0.1c0,0,0.1-1,0.7-2  c0.7-1.2,1.7-1.8,3-1.9h0.5V6.6h4.9v9.8h0.9c1.2,0.1,2.1,0.7,2.8,1.8c0.6,0.9,0.7,1.9,0.7,1.9l0.3,0l0.3,0c0,0-0.5-3.8-4-4.2V6.6  h9.5v14.3h-10v1.6h10v26.7H49.2v19.2c-0.1,0.2-0.2,0.4-0.3,0.5c-0.9,1.5-2.2,2.2-3.9,2.2v0.6c2.4,0,3.7-1.4,4.4-2.5  c0.2-0.3,0.3-0.5,0.4-0.8h9.6v-1.6h-8.6V50.8h21.9v16.1H65v1.6h7.7v24.9H44.2z"
				/>
				<rect id="room1" x="50" y="50" width="24" height="18" fill="red" fill-opacity="0.5" />
				<rect id="room2" x="44" y="68" width="30" height="26" fill="blue" fill-opacity="0.5" />
				<rect id="room3" x="36" y="28" width="38" height="22" fill="gold" fill-opacity="0.5" />
				<rect id="room4" x="36" y="6" width="21" height="22" fill="lime" fill-opacity="0.5" />
			</svg>
			<div class="textbox" id="text1">
				<b>This is a person</b>
				<p>They can't see inside the house</p>
			</div>
			<div class="textbox" id="text2">
				<b>Some other fact</b>
				<p>And more detail</p>
			</div>
			<div class="textbox" id="text3">
				<b>This is a plane</b>
				<p>It can fly, and tell things to this human</p>
			</div>
			<div id="picture-border" />
			<div id="room-chooser">
				<button class="room-button" style="border-color: red">Room 1</button>
				<button class="room-button" style="border-color: blue">Room 2</button>
				<button class="room-button" style="border-color: gold">Room 3</button>
				<button class="room-button" style="border-color: lime">Room 4</button>
			</div>
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
		position: absolute;
		padding: 0.2rem 1rem;
		border: 3px solid maroon;
		border-radius: 20px;
		opacity: 0;
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
	#svgs-container > svg {
		position: absolute;
		opacity: 0;
	}
	svg#house {
		bottom: 0;
		left: 50%;
		transform: translateX(-50%);
	}
	svg#sun {
		top: 0;
		right: 20%;
	}
	svg#person {
		bottom: 0;
		right: 30%;
		width: 100px;
		height: 100px;
	}
	svg#plane {
		bottom: 150px;
		left: 45%;
	}
	svg#dashes {
		bottom: 130px;
		left: 55%;
	}
	svg#moon {
		top: 0;
		right: 20%;
	}
	svg#bookcase {
		bottom: 0;
		right: 30%;
		height: 200px;
	}
	svg#floorplan {
		top: 0;
		right: -180px;
		height: 400px;
	}
	div#text1 {
		bottom: 150px;
		right: 10px;
		width: 150px;
	}
	div#text2 {
		bottom: 10px;
		right: 10px;
		width: 100px;
	}
	div#text3 {
		top: 80px;
		left: 20%;
	}
	div#picture-border {
		position: absolute;
		opacity: 0;
		top: 0;
		left: 0;
		right: 100px;
		bottom: 0;
		border: 2px solid silver;
		border-radius: 2px;
	}
	#room-chooser {
		position: absolute;
		opacity: 0;
		right: 100px;
		margin: 0.5rem;
	}
	button.room-button {
		background: #fff7;
		border: 1px solid black;
		border-radius: 3px;
		padding: 0.2rem 0.5rem;
	}
	#window {
		top: 40%;
		left: 10px;
	}
	#window-ai-highlight,
	#bookcase-ai-highlight {
		opacity: 0;
	}
</style>
