<script>
	import '$lib/styles/background-generator.css';
	import { onMount } from 'svelte';

	const { body } = document;
	let color1 = '#00ff00';
	let color2 = '#ff0000';
	let cssText = '';

	function setGradient() {
		body.style.background = `linear-gradient(to right, ${color1}, ${color2})`;

		cssText = body.style.background + ';';
	}

	setGradient();
</script>

<h1>Background Generator</h1>

<input type="color" name="color1" bind:value={color1} on:input={setGradient} />
<input type="color" name="color2" bind:value={color2} on:input={setGradient} />

<h2>Current CSS Background</h2>

<h3>{cssText}</h3>
