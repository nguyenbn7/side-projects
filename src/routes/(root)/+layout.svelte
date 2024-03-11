<script>
	import 'bootstrap/dist/css/bootstrap.min.css';
	import '$lib/styles/root-layout.css';

	import Navbar from '$lib/root/navbar.svelte';
	import Footer from '$lib/root/footer.svelte';

	import { setContext } from 'svelte';
	import { writable } from 'svelte/store';

	let searchText = '';
	const searchStore = writable(searchText);
	setContext('searchStore', searchStore);

	$: searchStore.set(searchText.toLowerCase().trim());
</script>

<div class="text-bg-dark h-100 d-flex" style="flex: 1">
	<div class="container d-flex mx-auto flex-column vh">
		<Navbar bind:searchText />
		<slot />
		<Footer />
	</div>
</div>
