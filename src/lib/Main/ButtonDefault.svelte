<script lang="ts">
	import ButtonIcon from '$lib/Main/ButtonIcon.svelte';
	import ButtonName from '$lib/Main/ButtonName.svelte';
	import ButtonState from '$lib/Main/ButtonState.svelte';
	import { createEventDispatcher } from 'svelte';

	export let entity_id: string;
	export let attributes: {[key: string]: any};
	export let template: any;
	export let loading: boolean;
	export let stateOn: boolean;

	export let sel: any;
	export let sectionName: string | undefined;
	export let entity: any;

	export let editMode: boolean = false;

	$: icon = (sel?.template?.icon && template?.icon?.output) || sel?.icon;
	$: color = (sel?.template?.color && template?.color?.output) || sel?.color;
	$: marquee = sel?.marquee;

	const dispatch = createEventDispatcher();

	function toggle() {
		dispatch('toggle');
	}

	function handleEvent(event: Event) {
		dispatch('handleEvent', { event });
	}
</script>

<div
	class="left"
	on:click|stopPropagation={(event) => {
		console.log('em', editMode);
		if (!editMode) {
			toggle();
		} else {
			handleEvent(event);
		}
	}}
	on:keydown
	role="button"
	tabindex="0"
>
	<ButtonIcon
		{icon}
		{entity_id}
		{color}
		{attributes}
		{template}
		{loading}
		{stateOn}
	/>
</div>

<div
	class="right"
	on:click|stopPropagation={handleEvent}
	on:keydown
	role="button"
	tabindex="0"
>
	<ButtonName
		{sel}
		{template}
		{sectionName}
		{entity}
		{stateOn}
	/>

	<ButtonState
		{sel}
		{template}
		{entity_id}
		{marquee}
		{stateOn}
	/>
</div>

<style>
	.left {
		display: inherit;
		padding: var(--container-padding);
	}

	.right {
		display: flex;
		flex-direction: column;
		justify-content: center;
		overflow: hidden;
		padding-right: var(--container-padding);
	}
</style>
