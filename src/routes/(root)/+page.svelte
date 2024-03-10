<script>
	import { base } from '$app/paths';
	import { APP_NAME } from '$lib/js/constants';
	import { getContext } from 'svelte';
	const searchStore = getContext('searchStore');

	const imageModules = import.meta.glob('$lib/assets/images/root/*.png', {
		eager: true,
		import: 'default'
	});

	const imagePath = Object.keys(imageModules)[0].split('/').slice(0, -1).join('/');

	const data = [
		{ link: 'quote-generator', name: 'Quote Generator', img: 'quote-generator.png' },
		{ link: 'pong', name: 'Pong', img: 'pong.png' },
		{ link: 'music-player', name: 'Music Player', img: 'music-player.png' },
		{ link: 'background-generator', name: 'Background Generator', img: 'background-generator.png' },
		{ link: 'infinity-scroll', name: 'Infinity Scroll', img: 'infinity-scroll.png' },
		{ link: 'calculator', name: 'Calculator', img: 'calculator.png' }
	];

	$: searchText = $searchStore;
	$: newData =
		searchText.length < 3 ? data : data.filter((d) => d.name.toLowerCase().includes(searchText));
</script>

<svelte:head>
	<title>{APP_NAME} | Home</title>
</svelte:head>

<main>
	<section class="pt-5 pb-3 text-center container">
		<div class="row py-lg-5">
			<div class="col-lg-6 col-md-8 mx-auto">
				<h1 class="fw-light">Projects</h1>
				<p class="lead">
					Websites collection built by HTML5, CSS and JavaScript vanilla - projects come from
					<br />
					<a
						href="https://zerotomastery.io/courses/javascript-projects/"
						class="link-info text-decoration-none"
					>
						JavaScript Web Projects: 20 Projects to Build Your Portfolio
					</a>
				</p>
				<p>
					<a
						href="https://github.com/nguyenbn7/small-websites"
						class="btn btn-lg btn-light fw-bold border-white bg-white"
					>
						Learn more
					</a>
				</p>
			</div>
		</div>
	</section>

	<div class="py-5">
		<div class="container-fluid">
			<div class="row row-cols-1 row-cols-lg-2 row-cols-xxl-3 g-3">
				{#each newData as item}
					<div class="col">
						<div class="card shadow-sm bg-dark-subtle p-0">
							{#if item.img}
								<img
									src={imageModules[`${imagePath}/${item.img}`] + ''}
									alt={item.name}
									class="bd-placeholder-img card-img-top image"
								/>
							{:else}
								<img
									src={imageModules[`${imagePath}/default.png`] + ''}
									alt={item.name}
									class="bd-placeholder-img card-img-top image"
								/>
							{/if}

							<div class="card-body">
								<p class="card-text text-center fw-bold">
									<a
										class="text-decoration-none text-primary"
										href="{base}/projects/{item.link}"
										data-sveltekit-reload
									>
										{item.name}
									</a>
								</p>
							</div>
						</div>
					</div>
				{/each}
			</div>
		</div>
	</div>
</main>

<style lang="scss">
	.card-img-top {
		width: 100%;
		height: 50vw;
		object-fit: cover;

		@media screen and (min-width: 992px) {
			height: 25vw;
		}

		@media screen and (min-width: 1200px) {
			height: 23vw;
		}

		@media screen and (min-width: 1400px) {
			height: 13vw;
		}
	}
</style>
