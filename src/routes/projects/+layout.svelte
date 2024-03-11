<script>
	import { page } from '$app/stores';
	import { APP_NAME } from '$lib/js/constants';
	import _ from 'lodash';

	const { startCase, toLower } = _;
	const projectName = startCase(toLower($page.url.pathname.split('/').pop()));
</script>

<a
	href="/"
	class="text-decoration-none position-fixed top-0 start-0"
	data-sveltekit-reload
	style="z-index: 1020; margin-top: 2em; margin-left: 2em;"
	title="Back"
>
	<i class="fa-solid fa-arrow-left fw-bold" style="font-size: 2em;"></i>
</a>

<svelte:head>
	<title>{APP_NAME} | {projectName}</title>
</svelte:head>

<slot />
