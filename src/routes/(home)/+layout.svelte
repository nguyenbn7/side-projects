<script>
	import 'bootstrap/dist/css/bootstrap.min.css';
	import Navbar from '$lib/home/navbar.svelte';
	import Footer from '$lib/home/footer.svelte';
</script>

<Navbar />
<slot />
<Footer />
