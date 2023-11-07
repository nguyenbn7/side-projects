<main>
	<section class="py-5 text-center container">
		<div class="row py-lg-5">
			<div class="col-lg-6 col-md-8 mx-auto">
				<h1 class="fw-light">Small Projects</h1>
				<p class="lead text-body-secondary">Contains 20 small side projects</p>
			</div>
		</div>
	</section>

	<div class="album py-5 bg-body-tertiary">
		<div class="container">
			<div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 g-3">
				{#each [...Array(20).keys()] as _}
					<div class="col">
						<div class="card shadow-sm">
							<svg
								class="bd-placeholder-img card-img-top"
								width="100%"
								height="300"
								xmlns="http://www.w3.org/2000/svg"
								role="img"
								aria-label="Placeholder: Thumbnail"
								preserveAspectRatio="xMidYMid slice"
								focusable="false"
								><title>Placeholder</title><rect width="100%" height="100%" fill="#55595c" /><text
									x="50%"
									y="50%"
									fill="#eceeef"
									dy=".3em">Thumbnail</text
								></svg
							>
						</div>
					</div>
				{/each}
			</div>
		</div>
	</div>
</main>
