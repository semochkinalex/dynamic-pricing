<script>
	import { tweened } from 'svelte/motion';
	import { cubicOut } from 'svelte/easing';

	
	let maxPrice = 2000;
	let minPrice = 1000; 
	
	let currentPrice = 1300;

	let amount = currentPrice;

	const currentPriceWidth = tweened((currentPrice - minPrice) / (maxPrice - minPrice) * 100, {
		duration: 400,
		easing: cubicOut
	});

	const askingPriceWidth = tweened((amount - minPrice) / (maxPrice - minPrice) * 100, {
		duration: 400,
		easing: cubicOut
	});

	$: status = !amount || amount < minPrice ? "error" : amount < minPrice ? "error" : amount < currentPrice ? "low" : amount === currentPrice ? "normal" : "best";

	$: {
		console.log(status);
		askingPriceWidth.set(amount < minPrice ? 0 : amount > maxPrice ? 100 : (amount - minPrice) / (maxPrice - minPrice) * 100);
	}

	$: {
		currentPriceWidth.set((currentPrice - minPrice) / (maxPrice - minPrice) * 100);
	}

</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="" />
</svelte:head>

<main class="main">
	<section class="content">
		<p class="title">Я готов заплатить:</p>
		
		<div class="inputs">
			<input min={1000} max={2000} class:indigo={status == "normal"} class:green={status == "best"} class:red={status == "error"} class:orange={status == "low"} bind:value={amount} class="input" type="number" placeholder="Сумма..." /> 
			<p class="currency">₽</p>
		</div>

		<div class="statistics" class:best={status == "best"} class:normal={status == "normal"} class:error={status == 'error'}>

			<div class="outer">
				<div class:higher={amount <= currentPrice} class="asking" style:width={`${$askingPriceWidth}%`}></div>
				<div class="current" style:width={`${$currentPriceWidth}%`}></div>
				<div class="benchmarks">
					<p class="benchmark">{minPrice} ₽</p>
					<p class="benchmark">{maxPrice} ₽</p>
				</div>
			</div>

		</div>
	</section>
</main>

<style>

	@import url('https://fonts.googleapis.com/css2?family=Inter&display=swap');

	* {
		font-family: 'Inter', sans-serif;
	}

	.content {
		min-width: 250px;;
	}

	.outer {
		height: 20px;
		position: relative;
		border-radius: 6px;
		background-color: rgb(255, 244, 230);
	}

	.current {
		position: absolute;
		height: 20px;
		border-radius: 6px 0 0 6px;
		z-index: 2;
		background-color: rgb(250, 176, 5);
	}

	.asking {
		position: absolute;
		z-index: 2;
		height: 20px;
		border-radius: 6px 0 0 6px;
		background-color: rgb(253, 126, 20);
	}

	.normal .outer {
		background-color: rgb(237, 242, 255);
	}

	.normal .asking {
		background-color: rgb(76, 110, 245);
	}

	.error .outer {
		background-color: rgb(255, 245, 245);
	}

	.error .current {
		background-color: rgb(250, 82, 82);
	}

	.benchmarks {
		position: absolute;
		display: flex;
		justify-content: space-between;
		width: 100%;
		top: 0;
		align-items: center;
		height: 20px;
		box-sizing: border-box;
		padding: 0 6px;
	}

	.benchmark {
		margin: 0;
		font-size: 10px;
		z-index: 3;
		color: black;
	}

	.higher {
		z-index: 3;
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

	.title {
		margin: 0 0 10px 0;
		font-size: 18px;
	}

	.inputs {
		position: relative;
		width: 100%;
	}

	.currency {
		margin: 0;
		position: absolute;
		top: 50%;
		transform: translate(0, -50%);
		right: 12px;
	}

	.input {
		width: 100%;
		box-sizing: border-box;
		color: black;
		border: 2px solid rgb(206, 212, 218);
		padding: 10px 10px;
		border-radius: 12px;

		outline: none;

		position: relative;
	}

	.input::placeholder {
		color: grey;
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

	.orange {
		border: 2px solid rgb(253, 126, 20);
	}

	.red {
		border: 2px solid rgb(250, 82, 82);
	}

	.green {
		border: 2px solid rgb(18, 184, 134);
	}
	.indigo {
		border: 2px solid rgb(76, 110, 245);
	}

	.best .outer {
		background-color: rgb(230, 252, 245);
	}
	.best .current {
		background-color: rgb(77, 220, 177);
	}
	.best .asking {
		background-color: rgb(18, 184, 134);
	}
</style>

