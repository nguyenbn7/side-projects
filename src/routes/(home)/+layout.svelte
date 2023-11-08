<script>
	import Navbar from '$lib/home/navbar.svelte';
	import Footer from '$lib/home/footer.svelte';
	import links from '$lib/links';
</script>

<svelte:head>
	<title>Side Projects</title>
</svelte:head>

<Navbar {links} />
<slot {links} />
<Footer />

<style>
	@import '~bootwatch/dist/darkly/bootstrap.min.css';
</style>
