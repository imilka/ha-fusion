<script lang="ts">
	import ButtonDefault from '$lib/Main/ButtonDefault.svelte';
	import { lang } from '$lib/Stores';
	import { getSupport } from '$lib/Utils';

	export let sel: any;
	export let attributes: { [key: string]: any };

	$: colSpan = sel?.col_span || 1;
	$: supported_features = attributes?.supported_features;
	$: supports = getSupport(supported_features, {
		TARGET_TEMPERATURE: 1,
		TARGET_TEMPERATURE_RANGE: 2,
		TARGET_HUMIDITY: 4,
		FAN_MODE: 8,
		PRESET_MODE: 16,
		SWING_MODE: 32,
		AUX_HEAT: 64
	});

	$: current_temperature = attributes?.current_temperature;
	$: target_temp_low = attributes?.target_temp_low;
	$: target_temp_high = attributes?.target_temp_high;
	$: fan_mode = attributes?.fan_mode;
	$: swing_mode = attributes?.swing_mode;
	$: hvac_mode = attributes?.hvac_mode;

	/**
	 * Determine additional info based on supported features
	 */
	let additional_info = null;
	let info_label = null;
	$: {
		if (colSpan === 2) {
			if (supports.TARGET_TEMPERATURE && current_temperature) {
				additional_info = `${current_temperature} °C`;
				info_label = $lang('current_temperature');
			} else if (supports.TARGET_TEMPERATURE_RANGE && target_temp_low && target_temp_high) {
				additional_info = `${target_temp_low} °C / ${target_temp_high} °C`;
				info_label = $lang('target_temperature');
			} else if (supports.FAN_MODE && fan_mode) {
				additional_info = $lang(fan_mode);
				info_label = $lang('fan_mode');
			} else if (supports.SWING_MODE && swing_mode) {
				additional_info = $lang(swing_mode);
				info_label = $lang('swing_mode');
			} else if (hvac_mode) {
				additional_info = $lang(hvac_mode);
				info_label = $lang('hvac_mode');
			}
		}
	}
</script>

<div class="climate-container" data-col-span={colSpan}>
	<div style="display: flex;">
		<ButtonDefault
			{...$$props}
			on:toggle
			on:handleEvent
		/>
	</div>

	{#if colSpan === 2 && additional_info}
		<div class="additional-info">
			<div class="info-label">{info_label}</div>
			<div class="info">{additional_info}</div>
		</div>
	{/if}
</div>

<style>
	.climate-container {
		display: flex;
		align-items: center;
		justify-content: space-between;
		flex: 1;
	}

	.additional-info {
		display: flex;
		flex-direction: column;
		text-align: right;
		font-size: 0.9rem;
		color: var(--theme-text-color);
		padding: var(--container-padding);
	}

	.additional-info .info-label {
		font-weight: 500;
		color: inherit;
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
		color: var(--theme-button-name-color-off);
		font-size: 0.95rem;
		margin-top: -1px;
	}

	.additional-info .info {
		grid-area: state;
		font-weight: 400;
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
		color: var(--theme-button-state-color-off);
		font-size: 0.925rem;
		margin-top: 1px;
	}
</style>
