<script lang="ts">
	import CurrencyList from './CurrencyList.svelte';

	let apiKey = 'cfa119cbaabf69fb4a42fdc1';
	let api = `https://v6.exchangerate-api.com/v6/${apiKey}/latest/`;

	// let { inputVal, inputFrom, inputTo, showInput, showFrom, showTo, exchange, outputStat } =
	// 	$props();

	let input = $state({
		value: ``, //valueInput
		from: ``, //fromConvert
		to: `` //toConvert
	});

	let show = $state({
		input: ``, //showInput
		from: ``, //showFrom
		to: `` //showTo
	});

	let display = $state({
		exchangeOut: 0,
		outputStatus: false
	});

	async function Convert() {
		if (isFinite(input.value) && input.value > 0 && input.from && input.to) {
			try {
				const response = await fetch(`${api}${input.from}`);
				const data = await response.json();
				if (data.conversion_rates) {
					let fromExchange = data.conversion_rates[input.from];
					let toExchange = data.conversion_rates[input.to];
					display.exchangeOut = (input.value / fromExchange) * toExchange;
					display.outputStatus = true;
					show.from = input.from;
					show.to = input.to;
					show.input = input.value;
				}
			} catch (error) {
				alert('Error fetching exchange rates:', error);
			}
		} else {
			alert('Invalid input or currency selection.');
		}
	}
</script>

<div>
	<div class="flex flex-col gap-4">
		<div class="flex flex-row gap-2">
			<input
				type="text"
				bind:value={input.value}
				placeholder="Enter amount to be converted"
				class="h-16 w-[52vw] rounded-2xl border-2 border-green-900 bg-transparent text-xl text-green-500 hover:border-green-500 focus:border-green-500 md:w-[320px]"
				onkeydown={(e) => e.key === `Enter` && Convert()}
			/>
			<button
				class="flex w-[75px] items-center justify-center rounded-2xl border-2 border-green-900 text-3xl text-green-600 hover:border-green-600 hover:bg-green-900"
				onclick={() => Convert()}>⇄</button
			>
		</div>
		<CurrencyList bind:selectTo={input.to} bind:selectFrom={input.from} />
	</div>
</div>
