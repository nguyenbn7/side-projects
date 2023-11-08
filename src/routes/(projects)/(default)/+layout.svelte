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
</svelte:head>
<slot />
