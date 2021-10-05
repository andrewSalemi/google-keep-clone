<script>
	export let name;
</script>

<main>
	<h1 class="heading-1">Hello <span class="heading-1__span">{name}!</span></h1>
	<p>Visit the <a href="https://svelte.dev/tutorial">Svelte tutorial</a> to learn how to build Svelte apps.</p>
</main>

<style lang="scss">
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	.heading-1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
		&__span {
			font-size: 9rem;
		}
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>