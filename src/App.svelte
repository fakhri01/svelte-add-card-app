<script>
	import Inputmask from "inputmask";
	import creditCardType from "credit-card-type";
	import Notification from "./Notification.svelte";

	let notificationHandler = false;

	let card = {
		number: "",
		name: "",
		expire: "",
		cvv: "",
	};

	let backface = false,
		numberInput,
		expireInput,
		cvvInput;

	$: if (numberInput && expireInput && cvvInput) {
		Inputmask({
			mask: "9999 9999 9999 9999",
		}).mask(numberInput);

		Inputmask({
			mask: "99/99",
		}).mask(expireInput);

		Inputmask({
			mask: "999",
		}).mask(cvvInput);
	}

	$: type = card.number ? creditCardType(card.number)?.[0]?.type : false;

	const addCard = () => {
		if (
			card.name.length > 0 &&
			card.number.length > 0 &&
			card.expire.length > 0 &&
			card.cvv.length > 0
		) {
			notificationHandler = true;
			console.log(card);
		}
	};
</script>


{#if notificationHandler}
	<Notification toggle />
{/if}

<div class="container">
	<h1>Card Details</h1>
	<div class="card" class:flip={backface}>
		<div class="front">
			<div class="top">
				<img src="img/chip.svg" alt="Chip" />
				{#if type}
					<img src="img/{type}.svg" alt="Visa" />
				{/if}
			</div>
			<div class="card-number">
				{card.number || "**** **** **** 1234"}
			</div>

			<div class="card-bottom">
				<div>
					<div class="key">Card Holder name</div>
					<div class="value">{card.name || "John Doe"}</div>
				</div>
				<div>
					<div class="key">Expiry Date</div>
					<div class="value">{card.expire || "02/30"}</div>
				</div>
			</div>
		</div>
		<div class="back">
			<div class="card-back">
				CVV <em>{card.cvv || "123"}</em>
			</div>
		</div>
	</div>
	<form on:submit|preventDefault>
		<label>Card Number</label>
		<input type="text" bind:this={numberInput} bind:value={card.number} />

		<label>Card Holder Name</label>
		<input type="text" bind:value={card.name} />
		<label>Expiring Date</label>
		<input type="text" bind:this={expireInput} bind:value={card.expire} />

		<label>CVV</label>
		<input
			class="last-input"
			type="text"
			on:focus={() => (backface = true)}
			on:blur={() => (backface = false)}
			bind:this={cvvInput}
			bind:value={card.cvv}
		/>

		<button class="submit-btn" on:click={addCard}>Save Card</button>
	</form>
</div>


<style>
	.card {
		width: 352px;
		height: 223px;
		position: relative;
		perspective: 800px;
	}

	.card .front,
	.card .back {
		width: inherit;
		height: inherit;
		display: flex;
		flex-direction: column;
		background: linear-gradient(31.58deg, #93278f -2.49%, #29abe2 67.92%);
		border-radius: 10px;
		position: absolute;
		top: 0;
		left: 0;
		backface-visibility: hidden;
		transition: 1s all;
		padding: 31px 27px;
	}
	.card .front .top {
		display: flex;
		justify-content: space-between;
	}

	.card .card-number {
		font-size: 2rem;
		color: #fff;
		letter-spacing: -1px;
		margin-top: 20px;
	}

	.card .front .card-bottom {
		margin-top: auto;
		display: flex;
		justify-content: space-between;
		color: #fff;
	}
	.card .front .card-bottom .key {
		font-size: 0.5rem;
		font-weight: 500;
		text-transform: uppercase;
		margin-bottom: 5px;
		opacity: 0.7;
	}

	.card .front .card-bottom .value {
		font-size: 1rem;
		font-weight: 600;
	}

	.card .card-back {
		background-color: #fff;
		padding: 20px;
		margin-top: auto;
		display: flex;
		justify-content: flex-end;
	}

	.card .back .card-back em {
		font-weight: bold;
		margin-left: 15px;
	}

	.card .back {
		transform: rotateY(180deg);
	}

	.card.flip .back {
		transform: rotateY(0);
	}
	.card.flip .front {
		transform: rotateY(-180deg);
	}
	form {
		display: flex;
		flex-direction: column;
	}
	form input {
		display: block;
		padding: 5px;
		margin-bottom: 1rem;
	}
	form .last-input {
		width: 100px;
	}

	form label {
		color: #2c3e50;
		font-weight: 600;
	}

	.submit-btn {
		height: 40px;
		background-color: #2c3e50;
		color: #fff;
		font-size: 1.2rem;
		font-weight: bold;
		cursor: pointer;
	}
</style>
