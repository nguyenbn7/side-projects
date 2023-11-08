<script>
	import { getContext } from 'svelte';
	const searchStore = getContext('searchStore');

	const data = [
		{ link: 'quote-generator', name: 'Quote Generator', img: 'quote-generator.png' },
		{ link: 'pong', name: 'Pong', img: 'pong.png' },
		{ link: 'music-player', name: 'Music Player', img: 'pong.png' },
		{ link: 'background-generator', name: 'Background Generator', img: 'background-generator.png' }
	];

	$: searchText = $searchStore;
	$: newData =
		searchText.length < 3 ? data : data.filter((d) => d.name.toLowerCase().includes(searchText));
</script>

<main>
	<section class="py-5 text-center container">
		<div class="row py-lg-5">
			<div class="col-lg-6 col-md-8 mx-auto">
				<h1 class="fw-light">Side Projects</h1>
				<p class="lead text-body-secondary">
					Minimum and small websites built by HTML5, CSS and JavaScript
					<span class="fw-bold text-danger">Powered By Svelte</span>
				</p>
			</div>
		</div>
	</section>

	<div class="album py-5 bg-body-tertiary">
		<div class="container">
			<div class="row row-cols-md-1 row-cols-lg-2 g-3">
				{#each newData as item}
					<div class="col">
						<div class="card shadow-sm">
							<div class="image position-relative">
								<img src="/{item.img}" alt={item.name} class="img-fluid" />
								<div class="d-flex align-items-center justify-content-center hover-overlay">
									<a
										class="text-decoration-none text-danger h-100 w-100 d-flex justify-content-center align-items-center"
										href="/{item.link}"
										data-sveltekit-reload
									>
										<span style="font-size: 3em;" class="fw-bold">{item.name}</span>
									</a>
								</div>
							</div>
						</div>
					</div>
				{/each}
			</div>
		</div>
	</div>
</main>

<style>
	.image :hover {
		cursor: pointer;
		opacity: 1;
	}

	.hover-overlay {
		position: absolute;
		width: 100%;
		height: 100%;
		top: 0;
		left: 0;
		background: rgba(255, 255, 255, 0.7);
		opacity: 0;
		transition: all 0.5s;
	}
</style>
