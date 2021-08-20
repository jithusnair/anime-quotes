<script>
	import { fade } from 'svelte/transition';

	let quotes = [];

	getRandomQuotes();

	let quote = '';
	let character = '';

	let pausePlay = 'Pause';

	let count = 0;

	let timer;

	$: if (quotes && quotes.length !== 0) {
		selectQuote();
		timer = setInterval(selectQuote, 15000);
	}

	function getRandomQuotes() {
		fetch('https://animechan.vercel.app/api/quotes')
			.then((response) => response.json())
			.then((quotesArr) => {
				quotes = quotesArr;
				quotes = [...quotes];
			});
	}

	function selectQuote() {
		quote = quotes[count].quote;
		character = quotes[count].character;
		count = count + 1;
		if (count === 10) {
			setTimeout(() => {
				getRandomQuotes();
				clearInterval(timer);
				count = 0;
			}, 4000);
		}
	}

	function toggle() {
		//  if playing
		if (pausePlay === 'Pause') {
			clearInterval(timer);
			pausePlay = 'Play';
		} else {
			selectQuote();
			timer = setInterval(selectQuote, 15000);
			pausePlay = 'Pause';
		}
	}
</script>

{#key quote}
	<div transition:fade>
		<p>{quote}</p>
		<caption>{character}</caption>
	</div>
{/key}

<button on:click={toggle}>{pausePlay}</button>

<style>
	@import url('https://fonts.googleapis.com/css2?family=Dancing+Script:wght@400;500;600;700&display=swap');

	div {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
	}

	p,
	caption {
		font-family: 'Dancing Script';
		font-size: 32px;
	}

	button {
		font-family: 'Dancing Script';
		font-weight: bold;
		font-size: 20px;
		background: transparent;
		position: absolute;
		right: 30px;
		bottom: 30px;
		outline: none;
		border: none;
		cursor: pointer;
		box-shadow: rgb(0 0 0 / 32%) 4px 4px 5px 3px;
		padding: 10px 24px;
		font-size: 18px;
		transition: all 0.3s ease;
		border-radius: 4px;
	}

	button:hover {
		box-shadow: rgb(0 0 0 / 32%) 3px 3px 7px -2px;
	}

	@media only screen and (max-width: 600px) {
		p,
		caption {
			font-family: 'Dancing Script';
			font-size: 16px;
		}

		button {
			font-size: 16px;
			padding: 8px 18px;
		}
	}
</style>
