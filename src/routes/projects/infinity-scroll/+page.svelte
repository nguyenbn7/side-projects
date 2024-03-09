<script>
	import loader from '$lib/assets/images/projects/infinity-scroll/loader.svg';
	import '$lib/styles/infinity-scroll.css';
	import { APP_NAME } from '$lib/js/constants';
	import { PUBLIC_UNSPLASH_API_KEY } from '$env/static/public';
	import { onMount } from 'svelte';

	const count = 20;
	const apiKey = PUBLIC_UNSPLASH_API_KEY;
	const apiURL = `https://api.unsplash.com/photos/random/?client_id=${apiKey}&count=${count}`;

	/**
	 * @type {any[]}
	 */
	let photos = [];
	let ready = false;
	/**
	 * @type {HTMLDivElement}
	 */
	let loaderNode;

	/**
	 * @param {TimerHandler} handler
	 * @param {number | undefined} ms
	 */
	async function timeout(handler, ms) {
		return new Promise((_) => setTimeout(handler, ms));
	}

	async function getPhotos() {
		try {
			ready = false;
			loaderNode.hidden = false;

			const response = await fetch(apiURL);
			const data = await response.json();

			photos = [...photos, ...data];
		} catch (error) {
			console.log(error);
		} finally {
			loaderNode.hidden = true;
			await timeout(() => {
				ready = true;
			}, 2000);
		}
	}

	async function loadNextImages() {
		if (window.innerHeight + window.scrollY >= document.body.offsetHeight - 1000 && ready) {
			await getPhotos();
		}
	}

	onMount(async () => {
		await getPhotos();
	});
</script>

<svelte:head>
	<title>{APP_NAME} | Infinity Scroll</title>
</svelte:head>

<svelte:window on:scroll={loadNextImages} />

<!-- Title -->
<h1>Unsplash API - Infinite Scroll</h1>

<!-- Loader -->
<div class="loader" id="loader" bind:this={loaderNode}>
	<img src={loader} alt="Loading" />
</div>

<!-- Image container -->
<div class="image-container" id="image-container">
	{#each photos as photo}
		<a href={photo.links.html} target="_blank">
			<img src={photo.urls.regular} alt={photo.alt_description} title={photo.alt_description} />
		</a>
	{/each}
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
