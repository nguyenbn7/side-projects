<script>
	function topFunction() {
		// TODO: add smooth to top 
		document.body.scrollTop = 0; // For Safari
		document.documentElement.scrollTop = 0; // For Chrome, Firefox, IE and Opera
	}
</script>

<footer class="text-body-secondary py-5">
	<div class="container">
		<p class="float-end mb-1">
			<!-- svelte-ignore a11y-invalid-attribute -->
			<a href="#" on:click={topFunction}>Back to top</a>
		</p>
		<p class="mb-1">
			Album example is © Bootstrap, but please download and customize it for yourself!
		</p>
		<p class="mb-0">
			New to Bootstrap? <a href="/">Visit the homepage</a> or read our
			<a href="/docs/5.3/getting-started/introduction/">getting started guide</a>.
		</p>
	</div>
</footer>
