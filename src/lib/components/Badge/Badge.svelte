<span {...$$restProps} class="label label-{color}" class:label-rounded={rounded}>
	<slot />
</span>

<script lang="ts" context="module">
	import type { Color } from '../../types/text';
</script>

<script lang="ts">
	export let color: Color = 'initial';
	export let rounded: boolean = false;
</script>

<style lang="scss">
	@import '@spectre-org/spectre-css/src/labels';
	:global(.spectre) {
		.label {
			font-size: 0.7rem;
			line-height: inherit;
			padding: 0.05rem 0.4rem 0.1rem;
			&.label-rounded {
				padding-left: 0.5rem;
				padding-right: 0.5rem;
			}
		}
	}
</style>
