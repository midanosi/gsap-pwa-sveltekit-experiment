<script lang="ts">
	export let width = '1.5rem';
	export let height = '1.5rem';
	export let focusable = 'false';
</script>

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
	{focusable}
	{width}
	{height}
>
	<path stroke="none" d="M0 0h24v24H0z" fill="none" />
	<circle cx="12" cy="12" r="4" />
	<path
		d="M3 12h1m8 -9v1m8 8h1m-9 8v1m-6.4 -15.4l.7 .7m12.1 -.7l-.7 .7m0 11.4l.7 .7m-12.1 -.7l-.7 .7"
	/>
</svg>
