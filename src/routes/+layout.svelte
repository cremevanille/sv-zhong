<script>
	import { base } from '$app/paths';
	import './layout.css';
	const { children } = $props();
</script>

<svelte:head>
	<link rel="icon" type="image/svg+xml" href="{base}/favicon.svg" />
	<link rel="preload" href="{base}/NotoSans.ttf" as="font" type="font/ttf" crossorigin="anonymous" />
	<link rel="preload" href="{base}/NotoSansSC.ttf" as="font" type="font/ttf" crossorigin="anonymous" />
</svelte:head>

<div class="icon" id="nav-icon">
	<svg viewBox="0 0 10 10">
		<path d="M 0 .5 H 10 M 0 3.5 H 10 M 0 6.5 H 10 M 0 9.5 H 10"/>
	</svg>
</div>
<nav>
	<ul>
		<li><a href="{base}/">Home</a></li>
		<li><a href="{base}/yinxixue">音系学</a></li>
		<li><a href="{base}/cihai">辞海</a></li>
	</ul>
</nav>
<main>
	{@render children?.()}
</main>

<style>
	#nav-icon { position: fixed; }
	.icon {
		height: 3rem;
		width: 3rem;

		display: grid;
		place-items: center;

		svg {
			height: 1rem;
			width: 1rem;

			fill: none;
			stroke: var(--red);
			stroke-width: 1;
		}
	}
	nav {
		position: fixed;
		top: 0;
		left: -10rem;

		height: 100vh;
		width: 10rem;
		z-index: 1;

		transition: left .3s ease-in-out;

		background-color: var(--white);

		padding-top: 5rem;
		border-right: var(--bold-border);
		box-sizing: border-box; 
	}
	:global body:has(#nav-icon:hover, nav:hover) {
		nav { left: 0; }
		main { filter: opacity(.3); }
	}
	main {
		margin: 0 4rem 0 4rem;
    	transition: filter .3s ease-in-out;
	}
</style>
