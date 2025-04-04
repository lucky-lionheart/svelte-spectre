{#if $$slots.label}
	<div class="col-4 col-{expand}-12">
		<label for="select-{fid}" class="form-label label-{size}">
			<slot name="label" />
		</label>
	</div>
{/if}

{#if multiple}
	<select
		multiple
		size={options.length}
		id="select-{fid}"
		class="form-select is-{validity} select-{size}"
		class:form-inline={inline}
		bind:value
		on:change
		on:click
		on:dblclick
		on:focus
		on:blur
		on:scroll
		on:keydown
	>
		{#if placeholder}
			<option value="">{placeholder}</option>
		{/if}
		{#each options as option, index}
			<option value={getValue(option, index)}>
				<slot name="option" {option}>{getLabel(option, index)}</slot>
			</option>
		{/each}
	</select>
{:else}
	<select
		{...$$restProps}
		id="select-{fid}"
		class="form-select is-{validity} select-{size}"
		class:form-inline={inline}
		bind:value
		on:change
		on:click
		on:dblclick
		on:focus
		on:blur
		on:scroll
		on:keydown
	>
		{#if placeholder}
			<option value="">{placeholder}</option>
		{/if}
		{#each options as option, index}
			<option value={getValue(option, index)}>
				<slot name="option" {option}>{getLabel(option, index)}</slot>
			</option>
		{/each}
	</select>
{/if}

<script lang="ts" context="module">
	import uuid from '../../helpers/uuid';

	import type { Size } from '../../types/size';

	type Validity = 'success' | 'error' | false;
	type Primitive = string | number | boolean;
	type Value = Primitive[] | Primitive | undefined;
	type Selected = number[] | number | undefined;
	type Option = Primitive | { value: Primitive; label?: string };

	export type { Size, Validity, Value, Option };
</script>

<script lang="ts">
	export let options: Option[] = [];
	export let value: Value;
	export let selected: Selected;
	export let inline = false;
	export let multiple = false;
	export let placeholder: string;
	export let size: Size;
	export let validity: Validity = false;
	export let expand: Size = 'xs';

	const fid: string = uuid();

	$: (selected = Array.isArray(value)
		? value.reduce<number[]>((selected, val) => {
				const i = getSelected(options, val);
				i !== undefined && selected.push(i);
				return selected;
		  }, [])
		: getSelected(options, value)),
		selected;

	function getSelected(options: Option[], value?: Option) {
		const i = options.findIndex((option, index) => getValue(option, index) === value);
		return i < 0 ? undefined : i;
	}

	function getLabel(option: Option, index: number) {
		return typeof option !== 'object' ? option : option.label || index + 1;
	}

	function getValue(option: Option, index: number) {
		return typeof option !== 'object' ? option : option.value || index;
	}
</script>

<style lang="scss">
	@import '@spectre-org/spectre-css/src/forms';
</style>
