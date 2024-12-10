<script lang="ts">
	import ButtonDefault from '$lib/Main/ButtonDefault.svelte';
	// export let attributes: { [key: string]: any };
	// $: temperature = attributes?.temperature || attributes?.current_temperature || 'N/A';
</script>

<div class="climate-container">
	<ButtonDefault
		{...$$props}
		on:toggle
		on:handleEvent
	/>

<!--	<div class="temperature">-->
<!--		{temperature}°C-->
<!--	</div>-->
</div>

<style>
	.climate-container {
		display: flex;
		align-items: center;
		justify-content: space-between;
	}

	/*.temperature {*/
	/*	font-size: 1rem;*/
	/*	font-weight: bold;*/
	/*	padding-left: 0.5rem;*/
	/*	color: var(--theme-text-color);*/
	/*}*/
</style>
