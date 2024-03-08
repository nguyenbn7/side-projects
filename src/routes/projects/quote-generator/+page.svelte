<script>
	import { onMount } from 'svelte';
	import '$lib/styles/quote-generator.css';
	import { localQuotes } from '$lib/js/quotes';

	/**
	 * @type {HTMLDivElement}
	 */
	let quoteContainer;
	/**
	 * @type {HTMLDivElement}
	 */
	let loader;
	/**
	 * @type {string}
	 */
	let quoteText;
	/**
	 * @type {string}
	 */
	let authorText;
	/**
	 * @type {string | any[]}
	 */
	let apiQuotes = [];
	let isQuoteLong = false;
	const quotesAPI = 'data/quotes.json';

	function loading() {
		loader.hidden = false;
		quoteContainer.hidden = true;
	}

	function complete() {
		quoteContainer.hidden = false;
		loader.hidden = true;
	}

	function getNewQuote() {
		loading();
		const quote = apiQuotes[Math.floor(Math.random() * apiQuotes.length)];
		if (quote.text.length > 120) {
			isQuoteLong = true;
		} else {
			isQuoteLong = false;
		}
		quoteText = quote.text;

		if (!quote.author) {
			authorText = 'Unknown';
		} else {
			authorText = quote.author;
		}
		complete();
	}

	onMount(async () => {
		loading();
		try {
			const response = await fetch(quotesAPI);
			apiQuotes = await response.json();
			getNewQuote();
		} catch (error) {
			console.log(error);
			apiQuotes = localQuotes;
			getNewQuote();
		}
	});

	function tweetQuote() {
		const twitterUrl = `https://twitter.com/intent/tweet?text=${quoteText} - ${authorText}`;
		window.open(twitterUrl, '_blank');
	}
</script>

<svelte:head>
	<title>Simple websites | Quote Generator</title>
</svelte:head>

<div class="quote-container" id="quote-container" bind:this={quoteContainer}>
	<!-- Quote -->
	<div class="quote-text">
		<i class="fa-solid fa-quote-left"> </i>
		<span
			id="quote"
			class:long-quote={isQuoteLong}
			bind:textContent={quoteText}
			contenteditable="true"
		/>
	</div>
	<!-- Author -->
	<div class="quote-author">
		<span id="author" bind:textContent={authorText} contenteditable="true" />
	</div>
	<!-- Buttons -->
	<div class="button-container">
		<button class="twitter-button" id="twitter" title="Tweet This!" on:click={tweetQuote}>
			<i class="fa-brands fa-twitter"></i>
		</button>
		<button id="new-quote" on:click={getNewQuote}>New Quote</button>
	</div>
</div>

<div class="loader" id="loader" bind:this={loader} />

<style>
	@import url('https://fonts.googleapis.com/css2?family=Playpen+Sans:wght@300&display=swap');

	.quote-container {
		width: auto;
		max-width: 900px;
		padding: 20px 30px;
		border-radius: 10px;
		background-color: rgba(255, 255, 255, 0.6);
		box-shadow: 0 10px 10px 10px rgba(0, 0, 0, 0.2);
	}

	.quote-text {
		font-size: 2.75rem !important;
	}

	.long-quote {
		font-size: 2rem;
	}

	.fa-quote-left {
		font-size: 4rem;
	}

	.quote-author {
		margin-top: 15px;
		font-size: 2rem;
		font-weight: 400;
		font-style: italic;
	}

	.button-container {
		margin-top: 15px;
		display: flex;
		justify-content: space-between;
	}

	button {
		cursor: pointer;
		font-size: 1.2rem;
		height: 2.5rem;
		border: none;
		border-radius: 10px;
		color: #fff;
		background: #333;
		outline: none;
		padding: 0.5rem 1.8rem;
		box-shadow: 0 0.3rem rgba(121, 121, 121, 0.65);
	}

	button:hover {
		filter: brightness(110%);
	}

	button:active {
		transform: translate(0, 0.3rem);
		box-shadow: 0 0.1rem rgba(255, 255, 255, 0.65);
	}

	.twitter-button:hover {
		color: #38a1f3;
	}

	.loader {
		border: 16px solid #f3f3f3;
		border-top: 16px solid #333;
		border-radius: 50%;
		width: 120px;
		height: 120px;
		animation: spin 2s linear infinite;
	}

	@keyframes spin {
		0% {
			transform: rotate(0deg);
		}

		100% {
			transform: rotate(360deg);
		}
	}

	/* Media Query */
	@media screen and (max-width: 1000px) {
		.quote-container {
			margin: auto 10px;
		}

		.quote-text {
			font-size: 2rem !important;
		}
	}
</style>
