<figure {...$$restProps} class="figure">
	<slot name="img">
		<img
			class="img-responsive"
			class:img-fit-contain={contain}
			class:img-fit-cover={cover}
			alt="figure via svelte-spectre"
			{...$$restProps}
		/>
	</slot>
	{#if $$slots.default}
		<figcaption class="figure-caption text-{align}"><slot /></figcaption>
	{/if}
</figure>

<script lang="ts" context="module">
	type Align = '' | 'left' | 'center' | 'right';
	export type { Align };
</script>

<script lang="ts">
	export let contain: boolean = false;
	export let cover: boolean = false;
	export let align: Align = 'center';
</script>

<style lang="scss">
	:global(.spectre) {
		@import '@spectre-org/spectre-css/src/media';
		.figure {
			margin: 0;
		}
	}
</style>
