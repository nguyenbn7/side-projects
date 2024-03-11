<script>
	import '$lib/styles/math-sprint-game.css';

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

	// Pages
	// const gamePage = document.getElementById('game-page');
	// const scorePage = document.getElementById('score-page');
	// const splashPage = document.getElementById('splash-page');
	// const countdownPage = document.getElementById('countdown-page');
	// // Splash Page
	// const startForm = document.getElementById('start-form');
	// const bestScores = document.querySelectorAll('.best-score-value');
	// // Countdown Page
	// const countdown = document.querySelector('.countdown');
	// // Game Page
	// const itemContainer = document.querySelector('.item-container');
	// // Score Page
	// const finalTimeEl = document.querySelector('.final-time');
	// const baseTimeEl = document.querySelector('.base-time');
	// const penaltyTimeEl = document.querySelector('.penalty-time');
	// const playAgainBtn = document.querySelector('.play-again');
	let hideSplashPage = false;
	let hideCountDownPage = true;
	let hideGamePage = true;
	let hideScorePage = true;

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

	function showCountDownPage() {
		hideCountDownPage = false;
		hideSplashPage = true;
		startCountDown(3);
		equationsArray = [...createEquations(questionAmount)];
		setTimeout(showGamePage, 400);
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
		console.log(playerGuessArray);
	}
</script>

<div class="game-container">
	<div class="header">
		<h1>Math Sprint Game</h1>
	</div>

	<div class="card" hidden={hideSplashPage}>
		<form id="start-form" on:submit|preventDefault={showCountDownPage}>
			<div class="selection-container">
				{#each [10, 25, 50, 99] as noQuestions}
					<div class="radio-container" class:selected-label={questionAmount === noQuestions}>
						<label for="value-{noQuestions}">{noQuestions} Questions</label>
						<input type="radio" name="questions" value={noQuestions} bind:group={questionAmount} />
						<span class="best-score">
							<span>Best Score</span>
							<span class="best-score-value">0.0s</span>
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

	<div class="card" id="game-page" hidden={hideGamePage}>
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
			<h1 class="final-time">3.4s</h1>
			<h1 class="base-time">Base Time: 2.4s</h1>
			<h1 class="penalty-time">Penalty: +1.0s</h1>
		</div>
		<div class="score-footer">
			<button class="play-again">Play Again</button>
		</div>
	</div>
</div>
