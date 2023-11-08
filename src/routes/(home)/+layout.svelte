<script>
	import Navbar from '$lib/home/navbar.svelte';
	import Footer from '$lib/home/footer.svelte';
	import { setContext } from 'svelte';
	import { writable } from 'svelte/store';

	let searchText = '';
	const searchStore = writable(searchText);
	setContext('searchStore', searchStore);

	$: searchStore.set(searchText.toLowerCase().trim());
</script>

<svelte:head>
	<title>Side Projects</title>
</svelte:head>

<Navbar bind:searchText />
<slot />
<Footer />

<style>
	@import '~bootwatch/dist/darkly/bootstrap.min.css';
</style>
