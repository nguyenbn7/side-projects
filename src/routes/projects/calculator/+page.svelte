<script>
	import '$lib/styles/calculator.css';

	/**
	 * @type {HTMLHeadingElement}
	 */
	let calculatorDisplay;
	let firstValue = 0;
	let operatorValue = '';
	let awaitingNextValue = false;

	/**
	 * @param {MouseEvent & { currentTarget: EventTarget & HTMLButtonElement; }} $event
	 */
	function onClickBtn($event) {
		const currentBtn = $event.currentTarget;

		if (currentBtn.classList.contains('operator')) {
			addOperator(currentBtn.value);
		} else if (currentBtn.classList.contains('decimal')) {
			addDecimal();
		} else if (currentBtn.classList.contains('clear')) {
			clearAll();
		} else {
			sendNumberValue(currentBtn.value);
		}
	}

	/**
	 * @param {string} number
	 */
	function sendNumberValue(number) {
		if (awaitingNextValue) {
			calculatorDisplay.textContent = number;
			awaitingNextValue = false;
			return;
		}
		const currentDisplayValue = calculatorDisplay.textContent;
		calculatorDisplay.textContent =
			currentDisplayValue === '0' ? number : currentDisplayValue + number;
	}

	function clearAll() {
		firstValue = 0;
		operatorValue = '';
		awaitingNextValue = false;
		calculatorDisplay.textContent = '0';
	}

	function addDecimal() {
		if (awaitingNextValue) return;

		if (!calculatorDisplay.textContent?.includes('.')) {
			calculatorDisplay.textContent = `${calculatorDisplay.textContent}.`;
		}
	}

	/**
	 * @param {string} operator
	 */
	function addOperator(operator) {
		const currentValue = Number(calculatorDisplay.textContent);

		if (operatorValue && awaitingNextValue) {
			operatorValue = operator;
			return;
		}

		if (!firstValue) {
			firstValue = currentValue;
		} else {
			const calculation = calculate[operatorValue](firstValue, currentValue);
			calculatorDisplay.textContent = '' + calculation;
			firstValue = calculation;
		}

		awaitingNextValue = true;
		operatorValue = operator;
	}

	/**
	 * @type {{[operator: string]: (first: number, second: number) => number}}
	 */
	const calculate = {
		'/': (/** @type {number} */ firstNumber, /** @type {number} */ secondNumber) =>
			firstNumber / secondNumber,
		'*': (/** @type {number} */ firstNumber, /** @type {number} */ secondNumber) =>
			firstNumber * secondNumber,
		'+': (/** @type {number} */ firstNumber, /** @type {number} */ secondNumber) =>
			firstNumber + secondNumber,
		'-': (/** @type {number} */ firstNumber, /** @type {number} */ secondNumber) =>
			firstNumber - secondNumber,
		'=': (/** @type {number} */ firstNumber, /** @type {number} */ secondNumber) => secondNumber
	};
</script>

<h1 class="text-white mb-5">Basic Calculator</h1>

<div class="calculator">
	<div class="calculator-display">
		<h1 bind:this={calculatorDisplay}>0</h1>
	</div>
	<div class="calculator-buttons">
		<button class="operator" value="+" on:click={onClickBtn}>+</button>
		<button class="operator" value="-" on:click={onClickBtn}>-</button>
		<button class="operator" value="*" on:click={onClickBtn}>×</button>
		<button class="operator" value="/" on:click={onClickBtn}>÷</button>
		<button value="7" on:click={onClickBtn}>7</button>
		<button value="8" on:click={onClickBtn}>8</button>
		<button value="9" on:click={onClickBtn}>9</button>
		<button value="4" on:click={onClickBtn}>4</button>
		<button value="5" on:click={onClickBtn}>5</button>
		<button value="6" on:click={onClickBtn}>6</button>
		<button value="1" on:click={onClickBtn}>1</button>
		<button value="2" on:click={onClickBtn}>2</button>
		<button value="3" on:click={onClickBtn}>3</button>
		<button class="decimal" value="." on:click={onClickBtn}>.</button>
		<button value="0" on:click={onClickBtn}>0</button>
		<button class="clear" value="C" on:click={onClickBtn}>C</button>
		<button class="equal-sign operator" value="=" on:click={onClickBtn}>=</button>
	</div>
</div>

<style lang="text/scss">

	.text-white {
		color: white;
	}

	.mb-5 {
		margin-bottom: 1.25em;
	}
	.calculator {
		background: white;
		width: 400px;
		border-radius: 12px;
		box-shadow: 0 5px 30px -5px rgba(0, 0, 0, 0.6);
	}

	.calculator-display {
		background: black;
		color: white;
		margin: 0;
		display: flex;
		align-items: center;
		justify-content: flex-end;
		border-radius: 10px 10px 0 0;
	}

	.calculator-display h1 {
		margin: 0;
		padding: 25px;
		font-size: 45px;
		font-family: 'Lucida Console', sans-serif;
		font-weight: 100;
		overflow-x: auto;
	}

	/* width */
	::-webkit-scrollbar {
		width: 10px;
	}

	/* Track */
	::-webkit-scrollbar-track {
		background: #f1f1f1;
	}

	/* Handle */
	::-webkit-scrollbar-thumb {
		background: #888;
	}

	/* Handle on hover */
	::-webkit-scrollbar-thumb:hover {
		background: #555;
	}

	.calculator-buttons {
		display: grid;
		grid-template-columns: repeat(4, 1fr);
		gap: 10px;
		padding: 10px;
	}

	button {
		min-height: 50px;
		font-size: 20px;
		font-weight: 450;
		border: none;
		border-radius: 5px;
		cursor: pointer;
		background: rgb(192, 191, 191);
	}

	button:hover {
		filter: brightness(110%);
	}

	button:active {
		transform: translateY(1px);
	}

	button:focus {
		outline: none;
	}

	.operator {
		background: grey;
		color: white;
		font-size: 30px;
	}

	.clear {
		background: rgb(224, 69, 69);
		color: white;
	}

	.clear:hover {
		filter: brightness(90%);
	}

	.equal-sign {
		grid-column: -2;
		grid-row: 2 / span 4;
		background: rgb(13, 206, 87);
	}

	@media screen and (max-width: 600px) {
		.calculator {
			width: 95%;
		}
	}
</style>
