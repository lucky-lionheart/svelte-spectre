<svelte:window on:keydown={open ? onKeydown : null} />

<div
	{...$$restProps}
	class:modal={!custom}
	class:active={custom || open}
	class="modal-{size}"
	id="modal-{mid}"
>
	{#if !custom}
		<span on:click={close} class="modal-overlay" aria-label="Close" />
	{/if}
	<div class="modal-container">
		<IconButton id="close" icon="cross" on:click={close} size={size === 'fs' ? 'xxl' : 'md'} />
		<div class="modal-header">
			<slot name="header" />
		</div>
		<div class="modal-body">
			<slot />
		</div>
		<div class="modal-footer">
			<slot name="footer" />
		</div>
	</div>
</div>

<script lang="ts" context="module">
	import { createEventDispatcher } from 'svelte';
	import IconButton from '../../components/Button/IconButton.svelte';
	import uuid from '../../helpers/uuid';

	export const SIZE = {
		sm: 320,
		md: 640,
		lg: 960,
		fs: 100,
	} as const;

	export type Size = keyof typeof SIZE;
</script>

<script lang="ts">
	export let custom: boolean = false;
	export let open: boolean = false;
	export let size: Size = 'md';
	export let onKeydown = (e: KeyboardEvent) => {
		if (e.key === 'Escape') {
			e.preventDefault();
			close(e);
		}
	};

	const mid: string = uuid();
	const dispatch = createEventDispatcher();

	function close(e: Event) {
		if (!custom) open = false;
		dispatch('close', e.target);
	}
</script>

<style lang="scss">
	:global(.spectre) {
		@import '@spectre-org/spectre-css/src/modals';
		@import '@spectre-org/spectre-css/src/animations';
		.modal.active {
			z-index: 500;
		}
		.modal-container {
			position: relative;
			max-height: 100% !important;
			:global(#close) {
				position: absolute !important;
				right: $unit-4;
				top: $unit-4;
				z-index: 1;
			}
			.modal-header {
				display: flex;
				justify-content: space-between;
				color: inherit;
			}
			.modal-header,
			.modal-footer {
				& > :global(*) {
					margin-bottom: 0 !important;
				}
			}
			.modal-body {
				flex: 1;
			}
		}

		.modal-sm {
			@extend .modal, .modal-sm;
		}
		.modal-lg {
			@extend .modal, .modal-lg;
			.modal-overlay {
				background: auto !important;
			}
		}
		.modal-fs {
			@extend .modal, .modal-lg;
			padding: 0 !important;
			.modal-container {
				max-width: 100% !important;
				max-height: 100%;
				width: 100%;
				height: 100%;
				justify-content: space-between;
			}
		}
	}
</style>
