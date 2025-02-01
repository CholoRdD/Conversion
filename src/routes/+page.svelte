<script lang="ts">
	import CurrencyList from './CurrencyList.svelte';
	import Header from './Header.svelte';
	import Output from './Output.svelte';

	let apiKey = 'cfa119cbaabf69fb4a42fdc1';
	let api = `https://v6.exchangerate-api.com/v6/${apiKey}/latest/`;

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

<div
	class="min-h-screen flex-row items-center justify-center bg-gradient-to-tr from-black to-green-900 md:flex"
>
	<div>
		<Header />
	</div>
	<div
		class="mx-8 flex w-[85vw] flex-row rounded-3xl border-2 border-green-700 bg-black bg-opacity-30 p-4 md:mx-10 md:my-12 md:h-[85vh] md:p-12"
	>
		<div class="flex flex-col items-center justify-center gap-5 md:flex-row">
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
							class="flex w-[75px] items-center justify-center rounded-2xl border-2 border-green-900 text-3xl text-green-600 hover:border-green-600 hover:font-bold hover:text-green-500"
							onclick={() => Convert()}>⇄</button
						>
					</div>
					<CurrencyList bind:selectTo={input.to} bind:selectFrom={input.from} />
				</div>
			</div>
			<Output
				from={show.from}
				to={show.to}
				amount={show.input}
				exchange={display.exchangeOut.toFixed(4)}
				hide={display.outputStatus}
			/>
		</div>
	</div>
</div>
