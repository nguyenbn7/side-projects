<script>
	/**
	 * @type {{link: string, name: string, img: string}[]}
	 */
	export let links;
</script>

<nav class="navbar navbar-expand-lg bg-primary">
	<div class="container-fluid">
		<div class="row w-100">
			<div class="col-3 col-md-6">
				<a class="navbar-brand ms-4 text-white" href="/">Side Projects</a>
			</div>
			<div class="col-9 col-md-6">
				<input
					class="form-control me-3"
					type="search"
					placeholder="Search project by name (Enter at least 3 characters)"
					aria-label="Search"
				/>
			</div>
		</div>
	</div>
</nav>
