<!-- CurrencyList.svelte -->

<script lang="ts">
	import { currency } from './CurrencyArray.js';

	export let selectFrom = currency[0];
	export let selectTo = currency[111];

	let searchFrom = '';
	let searchTo = '';

	// Filtered arrays for the button searching
	$: filterFrom = currency.filter((code) => code.toLowerCase().includes(searchFrom.toLowerCase()));
	$: filterTo = currency.filter((code) => code.toLowerCase().includes(searchTo.toLowerCase()));

	function selectBtn(type, code) {
		if (type === `from`) {
			selectFrom = code;
		} else if (type === `to`) {
			selectTo = code;
		}
	}
</script>

<div class="flex flex-row gap-2 md:gap-5">
	<div class="group">
		<div
			class="flex h-12 w-[35vw] flex-col flex-wrap items-center justify-center gap-1 overflow-scroll overflow-x-hidden
	rounded-t-2xl border-2 border-green-900 bg-transparent text-xl font-bold text-white group-hover:border-green-500 md:w-48"
		>
			FROM
		</div>
		<!-- Search Bar for FROM -->
		<input
			type="text"
			bind:value={searchFrom}
			placeholder="Search Currency"
			class="w-[35vw] border-2 border-green-900 bg-transparent p-2 text-center text-white group-hover:border-green-500 md:w-48"
		/>
		<div
			class="flex h-[25vh] w-[35vw] flex-wrap items-center justify-center gap-1 overflow-scroll overflow-x-hidden rounded-b-2xl
        border-2 border-green-900 bg-transparent text-white group-hover:border-green-500 md:h-[250px] md:w-48"
		>
			{#each filterFrom as code}
				<button
					class="h-10 w-full border-blue-800 hover:bg-green-600 hover:font-bold active:border-2 active:border-green-600 active:bg-green-400 {selectFrom ===
					code
						? `bg-green-600 font-bold`
						: ``} transition-all duration-200"
					on:click={() => selectBtn(`from`, code)}>{code}</button
				>
			{/each}
		</div>
	</div>

	<div class="group">
		<p
			class="flex h-12 w-[35vw] flex-wrap items-center justify-center gap-1 overflow-scroll overflow-x-hidden
	rounded-t-2xl border-2 border-green-900 bg-transparent text-xl font-bold text-white group-hover:border-green-500 md:w-48"
		>
			TO
		</p>
		<!-- Search Bar for TO -->
		<input
			type="text"
			bind:value={searchTo}
			placeholder="Search Currency"
			class="w-[35vw] border-2 border-green-900 bg-transparent p-2 text-center text-white group-hover:border-green-500 md:w-48"
		/>
		<div
			class="flex h-[25vh] w-[35vw] flex-wrap items-center justify-center gap-1 overflow-scroll overflow-x-hidden rounded-b-2xl
        border-2 border-green-900 bg-transparent text-white group-hover:border-green-500 md:h-[250px] md:w-48"
		>
			{#each filterTo as code}
				<button
					class="h-10 w-full border-blue-800 hover:bg-green-600 hover:font-bold active:border-2 active:border-green-600 active:bg-green-400 {selectTo ===
					code
						? `bg-green-600 font-bold`
						: ``} transition-all duration-200"
					on:click={() => selectBtn(`to`, code)}>{code}</button
				>
			{/each}
		</div>
	</div>
</div>
