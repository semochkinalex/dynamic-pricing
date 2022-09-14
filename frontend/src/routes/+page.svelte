<script>
	import { tweened } from 'svelte/motion';
	import { cubicOut } from 'svelte/easing';

	let amount;

	let maxPrice = 2000;
	let minPrice = 1000; 

	let currentPrice = 1200;

	const width = tweened((currentPrice - minPrice) / (maxPrice - minPrice) * 100, {
		duration: 400,
		easing: cubicOut
	});

	$: {
		width.set((currentPrice - minPrice) / (maxPrice - minPrice) * 100);
	}

</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="" />
</svelte:head>

<main class="main">
	<section>
		<p class="title">Ready to pay:</p>
		<input bind:value={amount} class="input" type="number" placeholder="Amount..." /> 

		<div class="statistics">
			<div class="benchmarks">
				<p class="benchmark">{minPrice} ₽</p>
				<p class="benchmark">{maxPrice} ₽</p>
			</div>

			<div class="outer"><div class="inner" style:width={`${$width}%`}></div></div>
			<div class="benchmarks">
				<p class="benchmark">min. price</p>
				<p class="benchmark">max. price</p>
			</div>
		</div>
	</section>
</main>

<style>
	@import url('https://fonts.googleapis.com/css2?family=Inter&display=swap');

	* {
		font-family: 'Inter', sans-serif;
	}

	.outer {
		height: 15px;
		border-radius: 4px;
		background-color: #D9D9D9;
	}

	.inner {
		/* width: 20%; */
		height: 15px;
		border-radius: 4px;
		background-color: #808080;
	}

	.main {
		width: 100%;
		height: 100vh;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}

	.statistics {
		margin: 10px 0;
		display: flex;
		flex-direction: column;
		gap: 5px;
	}

	.benchmarks {
		display: flex;
		justify-content: space-between;
	}

	.benchmark {
		font-size: 12px;
		margin: 0;
	}

	.title {
		font-size: 24px;
		font-weight: bolder;
	}

	.input {
		padding: 20px;
		border-radius: 12px;
		
		background-color: #D9D9D9;

		border: none;
		outline: none;
	}

	input::-webkit-outer-spin-button,
	input::-webkit-inner-spin-button {
  		-webkit-appearance: none;
  		margin: 0;
	}

	/* Firefox */
	input[type=number] {
	  -moz-appearance: textfield;
	}
</style>

