<!-- Output.svelte -->

<script>
	let { from, to, amount, exchange, hide } = $props();
</script>

<div
	class="flex h-[120px] w-[75vw] flex-row items-center justify-center gap-4 overflow-hidden rounded-2xl border-2 border-green-900 bg-transparent text-2xl font-bold text-white md:h-[70vh] md:w-[48vw] md:flex-col md:gap-8 md:text-7xl"
>
	<div class="{hide ? 'flex flex-row' : 'hidden'} gap-2 md:gap-5">
		<p>{amount}</p>
		<p class="flex items-end justify-center font-mono text-lg md:text-5xl">{from}</p>
	</div>

	<p class="md:{hide ? 'flex flex-row' : 'hidden'} hidden">↓</p>

	<p class="{hide ? 'flex flex-row' : 'hidden'} md:hidden">→</p>

	<div class="{hide ? 'flex flex-row' : 'hidden'} gap-2 md:gap-5">
		<p>{exchange}</p>
		<p class="flex items-end justify-center font-mono text-lg md:text-5xl">{to}</p>
	</div>
	<p
		class="{hide
			? 'hidden'
			: 'flex flex-row'} items-center justify-center text-center text-xl font-thin md:text-4xl"
	>
		PLEASE INPUT AMOUNT AND CURRENCIES TO BE CONVERTED
	</p>
</div>
