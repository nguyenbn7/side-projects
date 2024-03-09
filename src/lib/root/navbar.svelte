<script>
	import logo from '$lib/assets/images/logo.png';
	/**
	 * @type {string}
	 */
	export let searchText;
</script>

<nav class="navbar navbar-expand-lg">
	<div class="row w-100 align-items-center">
		<div class="col-3 col-md-6 text-start">
			<a class="navbar-brand d-flex align-items-center text-white" href="/">
				<img src={logo} alt="Logo" style="width: 1.75em; height: 1.75em;" class="me-2" />Simple
				websites
			</a>
		</div>
		<div class="col-9 col-md-6">
			<input
				class="form-control me-3"
				type="text"
				placeholder="Enter at least 3 characters"
				aria-label="Search"
				bind:value={searchText}
				contenteditable="true"
				maxlength="100"
			/>
		</div>
	</div>
</nav>
