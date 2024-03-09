<script>
	import loader from '$lib/assets/images/projects/infinity-scroll/loader.svg';
	import '$lib/styles/infinity-scroll.css';
	import { APP_NAME } from '$lib/js/constants';
</script>

<svelte:head>
	<title>{APP_NAME} | Infinity Scroll</title>
</svelte:head>

<!-- Title -->
<h1>Unsplash API - Infinite Scroll</h1>

<!-- Loader -->
<div class="loader" id="loader">
	<img src={loader} alt="Loading" />
</div>
