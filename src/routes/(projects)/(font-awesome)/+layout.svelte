<script>
	import { page } from '$app/stores';

	const pageName = $page.url.pathname
		.substring(1)
		.split('-')
		.map((txt) => txt.charAt(0).toUpperCase() + txt.substring(1).toLowerCase())
		.join(' ');
</script>

<svelte:head>
	<title>Side Projects - {pageName}</title>
	<link
		rel="stylesheet"
		href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.10.2/css/all.min.css"
	/>
</svelte:head>
<a
	href="/"
	data-sveltekit-reload
	style="text-decoration: none; position: fixed; top: 0; z-index: 1020; left: 0; margin-top: 2em; margin-left: 2em;"
	title="Back"
>
	<img src="arrow-return-left.svg" alt="Back" style="height: 4em; background: white;" />
</a>
<slot />
