<script>
	let { children } = $props();
</script>

<div id="body" style="display: contents">
	<div class="icon" id="nav-icon">
		<svg viewBox="0 0 10 10">
			<path d="M 0 .5 H 10 M 0 3.5 H 10 M 0 6.5 H 10 M 0 9.5 H 10"/>
		</svg>
	</div>
	<nav>
		<ul>
			<li><a href="/">Home</a></li>
			<li><a href="/yinxixue">yinxixue</a></li>
			<li><a href="/cihai">cihai</a></li>
		</ul>
	</nav>
	<main>
		{@render children?.()}
	</main>
</div>

<style>
	:root { 
		--black: #2c0006;
		--grey: #989593;
		--white: #faf9f3;
		--red: #b82132;

		--light: 200;
		--regular: 400;
		--bold: 600;

		--border-size: 2px;
		--border: var(--border-size) solid var(--red);
		--bold-border-size: 3px;
		--bold-border: var(--bold-border-size) solid var(--red);

		color: var(--black);
		background-color: var(--white);

		font-size: 20px; 
	}

	#body:has(#menu:hover, nav:hover) {
		nav { left: 0; }
		main { filter: opacity(.3); }
	}

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
	#nav-icon { position: fixed; }

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

	#nav-icon:hover + nav,
	nav:hover { left: 0; }
	#nav-icon:hover ~ main,
	nav:hover ~ main { filter: opacity(.3); }

	main {
		margin: 0 4rem 0 4rem;
    	transition: filter .3s ease-in-out;
	}
</style>
