<script>
	import { page } from '$app/stores';
	import { APP_NAME } from '$lib/js/constants';
	import _ from 'lodash';

	const { startCase, toLower } = _;
	const projectName = startCase(toLower($page.url.pathname.split('/').pop()));
</script>

<a href="/" class="back-btn" data-sveltekit-reload title="Back">
	<i class="fa-solid fa-arrow-left fw-bold" style="font-size: 2em;"></i>
</a>

<svelte:head>
	<title>{APP_NAME} | {projectName}</title>
</svelte:head>

<slot />

<style>
	.back-btn {
		position: fixed;
		top: 0;
		left: 0;
		text-decoration: none;
		z-index: 1020;
		margin-top: 2em;
		margin-left: 2em;
	}
</style>
