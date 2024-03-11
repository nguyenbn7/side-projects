<script>
	import '$lib/styles/math-sprint-game.css';
	import { onMount } from 'svelte';

	/**
	 * Get random number up to a max number
	 * @param {number} max
	 */
	function getRandomInt(max) {
		return Math.floor(Math.random() * Math.floor(max));
	}

	/**
	 * https://stackoverflow.com/questions/2450954/how-to-randomize-shuffle-a-javascript-array
	 * @param {any[]} array
	 */
	function shuffle(array) {
		let currentIndex = array.length,
			randomIndex;

		// While there remain elements to shuffle.
		while (currentIndex > 0) {
			// Pick a remaining element.
			randomIndex = Math.floor(Math.random() * currentIndex);
			currentIndex--;

			// And swap it with the current element.
			[array[currentIndex], array[randomIndex]] = [array[randomIndex], array[currentIndex]];
		}

		return array;
	}

	/**
	 * Count from seconds to 1 and final print GO!
	 * @param {number} seconds
	 */
	function startCountDown(seconds) {
		let counter = seconds;
		const secondTotalAmount = (seconds + 1) * 1000;
		countdown.textContent = '' + counter;

		const interval = setInterval(() => {
			counter--;

			countdown.textContent = '' + counter;

			if (counter < 1) {
				clearInterval(interval);
				countdown.textContent = 'GO!';
			}
		}, 1000);

		return secondTotalAmount;
	}

	let hideSplashPage = false;
	let hideCountDownPage = true;
	let hideGamePage = true;
	let hideScorePage = true;
	/**
	 * @type {HTMLDivElement}
	 */
	let gamePage;
	let hidePlayAgainBtn = true;

	/**
	 * @type {HTMLHeadingElement}
	 */
	let countdown;
	/**
	 * @type {HTMLDivElement}
	 */
	let itemContainer;

	let questionAmount = 0;
	/**
	 * @type {{ value?: any; evaluated?: string | undefined; }[]}
	 */
	let equationsArray = [];
	/**
	 * @type {string[]}
	 */
	let playerGuessArray = [];
	let valueY = 0;

	/**
	 * @type {number | undefined}
	 */
	let timer;
	let timePlayed = 0;
	let penaltyTime = 0;
	let finalTime = 0;
	/**
	 * @type {{questions: number, bestScore: string}[]}
	 */
	let bestScoreArray = [];

	function checkTime() {
		if (playerGuessArray.length >= questionAmount) {
			clearInterval(timer);
			equationsArray.forEach((equation, idx) => {
				if (equation.evaluated === playerGuessArray[idx]) {
				} else {
					penaltyTime += 0.5;
				}
			});
			finalTime = timePlayed + penaltyTime;
			updateBestScore();
			hideGamePage = true;
			hideScorePage = false;
			setTimeout(() => (hidePlayAgainBtn = false), 1000);
			itemContainer.scrollTo({ top: 0, behavior: 'instant' });
		}
	}

	function addTimer() {
		timePlayed += 0.1;
		checkTime();
	}

	/**
	 * @param {Event & { currentTarget: EventTarget & HTMLDivElement; }} $event
	 */
	// @ts-ignore
	function startTimer($event) {
		timePlayed = 0;
		penaltyTime = 0;
		finalTime = 0;
		timer = setInterval(addTimer, 100);
		// @ts-ignore
		return $event.currentTarget.removeEventListener('click', startTimer);
	}

	function showCountDownPage() {
		hideCountDownPage = false;
		hideSplashPage = true;
		const timeBeforeGame = startCountDown(3);
		equationsArray = [...createEquations(questionAmount)];
		setTimeout(showGamePage, timeBeforeGame);
	}

	function showGamePage() {
		hideCountDownPage = true;
		hideGamePage = false;
	}

	/**
	 * Create Correct/Incorrect Random Equations
	 * @param {number} questionAmount
	 * @returns {{ value?: any; evaluated?: string; }[]}
	 */
	function createEquations(questionAmount) {
		let firstNumber = 0;
		let secondNumber = 0;
		/**
		 * @type {{ value?: any; evaluated?: string; }}
		 */
		let equationObject = {};
		/**
		 * @type {string[]}
		 */
		const wrongFormat = [];

		// Randomly choose how many correct equations there should be
		const correctEquations = getRandomInt(questionAmount);
		// Set amount of wrong equations
		const wrongEquations = questionAmount - correctEquations;

		// Loop through, multiply random numbers up to 9, push to array
		for (let i = 0; i < correctEquations; i++) {
			firstNumber = getRandomInt(9);
			secondNumber = getRandomInt(9);
			const equationValue = firstNumber * secondNumber;
			const equation = `${firstNumber} x ${secondNumber} = ${equationValue}`;
			equationObject = { value: equation, evaluated: 'true' };
			equationsArray.push(equationObject);
		}

		// Loop through, mess with the equation results, push to array
		for (let i = 0; i < wrongEquations; i++) {
			firstNumber = getRandomInt(9);
			secondNumber = getRandomInt(9);
			const equationValue = firstNumber * secondNumber;
			wrongFormat[0] = `${firstNumber} x ${secondNumber + 1} = ${equationValue}`;
			wrongFormat[1] = `${firstNumber} x ${secondNumber} = ${equationValue - 1}`;
			wrongFormat[2] = `${firstNumber + 1} x ${secondNumber} = ${equationValue}`;
			const formatChoice = getRandomInt(3);
			const equation = wrongFormat[formatChoice];
			equationObject = { value: equation, evaluated: 'false' };
			equationsArray.push(equationObject);
		}
		return shuffle(equationsArray);
	}

	/**
	 * @param {boolean} guessesTrue
	 */
	function select(guessesTrue) {
		valueY += 80;
		itemContainer.scroll(0, valueY);
		playerGuessArray = [...playerGuessArray, guessesTrue ? 'true' : 'false'];
	}

	function playAgain() {
		// @ts-ignore
		gamePage.addEventListener('click', startTimer);
		hideScorePage = true;
		hideSplashPage = false;
		equationsArray = [];
		playerGuessArray = [];
		valueY = 0;
		hidePlayAgainBtn = true;
	}

	function getSavedBestScores() {
		const value = localStorage.getItem('bestScores');

		if (value) {
			bestScoreArray = JSON.parse(value);
			return;
		}

		bestScoreArray = [
			{ questions: 10, bestScore: finalTime.toFixed(1) },
			{ questions: 25, bestScore: finalTime.toFixed(1) },
			{ questions: 50, bestScore: finalTime.toFixed(1) },
			{ questions: 99, bestScore: finalTime.toFixed(1) }
		];

		localStorage.setItem('bestScores', JSON.stringify(bestScoreArray));
	}

	function updateBestScore() {
		bestScoreArray.forEach((score, idx) => {
			if (score.questions === questionAmount) {
				const savedBestScore = Number(bestScoreArray[idx].bestScore);
				if (savedBestScore === 0 || savedBestScore > finalTime) {
					bestScoreArray[idx].bestScore = finalTime.toFixed(1);
				}
			}
		});
		localStorage.setItem('bestScores', JSON.stringify(bestScoreArray));
	}

	onMount(() => {
		getSavedBestScores();
	});
</script>

<div class="game-container">
	<div class="header">
		<h1>Math Sprint Game</h1>
	</div>

	<div class="card" hidden={hideSplashPage}>
		<form id="start-form" on:submit|preventDefault={showCountDownPage}>
			<div class="selection-container">
				{#each [10, 25, 50, 99] as noQuestions, idx}
					<div class="radio-container" class:selected-label={questionAmount === noQuestions}>
						<label for="value-{noQuestions}">{noQuestions} Questions</label>
						<input type="radio" name="questions" value={noQuestions} bind:group={questionAmount} />
						<span class="best-score">
							<span>Best Score</span>
							<span class="best-score-value">{bestScoreArray[idx]?.bestScore}s</span>
						</span>
					</div>
				{/each}
			</div>
			<div class="selection-footer">
				<button type="submit" class="start">Start Round</button>
			</div>
		</form>
	</div>

	<div class="card" hidden={hideCountDownPage}>
		<h1 class="countdown" bind:this={countdown}>{''}</h1>
	</div>

	<!-- svelte-ignore a11y-click-events-have-key-events -->
	<!-- svelte-ignore a11y-no-static-element-interactions -->
	<div class="card" hidden={hideGamePage} on:click={startTimer} bind:this={gamePage}>
		<div class="item-container" bind:this={itemContainer}>
			<div class="height-240"></div>
			<div class="selected-item"></div>
			{#each equationsArray as equation}
				<div class="item">
					<h1>{equation.value}</h1>
				</div>
			{/each}
			<div class="height-500"></div>
		</div>
		<div class="item-footer">
			<button class="wrong" on:click={() => select(false)}>Wrong</button>
			<button class="right" on:click={() => select(true)}>Right</button>
		</div>
	</div>

	<div class="card" hidden={hideScorePage}>
		<div class="score-container">
			<h1 class="title">Your Time</h1>
			<h1 class="final-time">{finalTime.toFixed(1)}s</h1>
			<h1 class="base-time">Base Time: {timePlayed.toFixed(1)}s</h1>
			<h1 class="penalty-time">Penalty: +{penaltyTime.toFixed(1)}s</h1>
		</div>
		<div class="score-footer">
			<button class="play-again" on:click={playAgain} hidden={hidePlayAgainBtn}>Play Again</button>
		</div>
	</div>
</div>
