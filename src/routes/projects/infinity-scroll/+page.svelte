<script>
	import loader from '$lib/assets/images/projects/infinity-scroll/loader.svg';
	import '$lib/styles/infinity-scroll.css';
	import { APP_NAME } from '$lib/js/constants';
	import { PUBLIC_UNSPLASH_API_KEY } from '$env/static/public';
	import { onMount } from 'svelte';

	const count = 10;
	const apiKey = PUBLIC_UNSPLASH_API_KEY;
	const apiURL = `https://api.unsplash.com/photos/random/?client_id=${apiKey}&count=${count}`;

	async function getPhotos() {
		try {
			const response = await fetch(apiURL);
			const data = await response.json();
			console.log(data);
		} catch (error) {
			console.log(error);
		}
	}

	onMount(async () => {
		await getPhotos();
	});
</script>

<svelte:head>
	<title>{APP_NAME} | Infinity Scroll</title>
</svelte:head>

<!-- Title -->
<h1>Unsplash API - Infinite Scroll</h1>

<!-- Loader -->
<div class="loader" id="loader" hidden>
	<img src={loader} alt="Loading" />
</div>

<!-- Image container -->
<div class="image-container" id="image-container">
	<img
		src="https://images.unsplash.com/photo-1667236978744-92b4599d0f94?q=80&w=1974&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D"
		alt=""
	/>
	<img
		src="https://images.unsplash.com/photo-1667236978744-92b4599d0f94?q=80&w=1974&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D"
		alt=""
	/>
	<img
		src="https://images.unsplash.com/photo-1667236978744-92b4599d0f94?q=80&w=1974&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D"
		alt=""
	/>
</div>

<style lang="scss">
	.loader {
		position: fixed;
		top: 0;
		bottom: 0;
		left: 0;
		right: 0;
		background: rgba(255, 255, 255, 0.8);

		& img {
			position: fixed;
			top: 50%;
			left: 50%;
			transform: translate(-50%, -50%);
		}
	}

	.image-container {
		margin: 10px 30%;

		& img {
			width: 100%;
			margin-top: 5px;
		}
	}

	@media screen and (max-width: 600px) {
		h1 {
			font-size: 20px;
		}

		.image-container {
			margin: 10px;
		}
	}
</style>
