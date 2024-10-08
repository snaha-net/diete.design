<script lang="ts">
	import { Checkmark } from 'carbon-icons-svelte'
	import type { SelectStore } from './select-store.svelte'
	import { getContext } from 'svelte'
	import type { HTMLButtonAttributes } from 'svelte/elements'

	interface Props extends HTMLButtonAttributes {
		value: string
	}
	let { value, children, class: className = '', ...restProps }: Props = $props()

	const store = getContext<SelectStore>('select-store')

	let button = $state<HTMLButtonElement | undefined>()

	$effect(() => {
		store.registerValue(value, button?.innerText)
	})
	let marked = $derived(store.marked === value)
	let selected = $derived(store.value === value)

	let size: 16 | 24 | 32 = $derived(store.size === 'large' ? 32 : store.size === 'small' ? 16 : 24)
</script>

<button
	class="ghost {store.size} {className}"
	bind:this={button}
	onclick={() => {
		if (!store.open) return
		store.marked = value
		store.value = value
	}}
	onmouseenter={() => {
		store.marked = value
	}}
	class:selected
	class:marked
	tabindex="-1"
	{...restProps}
>
	{#if children}
		{@render children()}
	{:else}
		{value}
	{/if}
	{#if selected}
		<Checkmark {size} />
	{/if}
</button>

<style lang="postcss">
	button {
		display: inline-flex;
		justify-content: space-between;
		align-items: center;
		gap: 0.5rem;
		cursor: pointer;
		border-radius: var(--border-radius);
		font-style: normal;
		font-weight: 400;
		font-family: var(--font-family-sans-serif);
		&:disabled {
			opacity: 0.25;
			cursor: not-allowed;
		}
	}
	.ghost {
		border: 1px solid transparent;
		background: transparent;
		color: var(--colors-ultra-high);
		&.marked:not(:disabled),
		&.marked:not(:disabled).selected:not(:disabled) {
			background: var(--colors-low);
		}
	}
	.default {
		padding: var(--three-quarters-padding);
		min-width: 3rem;
		font-size: var(--font-size);
		line-height: var(--line-height);
		letter-spacing: var(--letter-spacing);
	}
	.large {
		padding: var(--three-quarters-padding);
		min-width: 3.5rem;
		font-size: var(--font-size-large);
		line-height: var(--line-height-large);
		letter-spacing: var(--letter-spacing-large);
	}
	.compact {
		padding: var(--half-padding);
		min-width: 2.5rem;
		font-size: var(--font-size);
		line-height: var(--line-height);
		letter-spacing: var(--letter-spacing);
	}
	.small {
		gap: 0.25rem;
		padding: var(--half-padding);
		min-width: 2rem;
		font-size: var(--font-size-small);
		line-height: var(--line-height-small);
		letter-spacing: var(--letter-spacing-small);
	}
</style>
