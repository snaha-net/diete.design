<script lang="ts">
	import { Add, Subtract } from 'carbon-icons-svelte'
	import Button from '../button.svelte'
	import Input, { type Props } from './input.svelte'
	import type { HTMLInputAttributes } from 'svelte/elements'
	interface CustomProps extends Props {
		showControls?: boolean
	}
	let {
		value = $bindable(0),
		showControls = false,
		dimension,
		disabled,
		...restProps
	}: CustomProps & Omit<HTMLInputAttributes, 'type'> = $props()
	let size: 16 | 24 | 32 = $derived(dimension === 'large' ? 32 : dimension === 'small' ? 16 : 24)
</script>

{#snippet buttons()}
	<Button {dimension} {disabled} variant="secondary" onclick={() => (value -= 1)}>
		<Subtract {size} />
	</Button>
	<Button {dimension} {disabled} variant="secondary" onclick={() => (value += 1)}>
		<Add {size} />
	</Button>
{/snippet}

<Input
	bind:value
	{dimension}
	{disabled}
	buttons={showControls ? buttons : undefined}
	{...restProps}
	type="number"
/>
