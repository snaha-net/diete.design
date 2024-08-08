<script lang="ts">
	import { Add, Subtract } from 'carbon-icons-svelte'
	import Button from '../button.svelte'
	import Input, { type Props } from './input.svelte'
	import type { HTMLInputAttributes } from 'svelte/elements'
	interface CustomProps extends Props {
		formatter?: Intl.NumberFormat
	}
	let {
		formatter,
		value = $bindable(0),
		dimension,
		disabled,
		...restProps
	}: CustomProps & Omit<HTMLInputAttributes, 'type'> = $props()
	let size: 16 | 24 | 32 = $derived(dimension === 'large' ? 32 : dimension === 'small' ? 16 : 24)

	let stringValue = $state(numberToLocalString(value, formatter))

	function numberToLocalString(value: number, formatter?: Intl.NumberFormat): string | undefined {
		return formatter ? formatter.format(value) : value.toString()
	}

	function localeStringToNumber(
		numberString: string,
		formatter?: Intl.NumberFormat,
	): number | undefined {
		const regex = /^-?([0-9]+(,+[0-9]+)?(\.[0-9]+)?|(\.|,)[0-9]+)$/g

		if (!formatter) {
			if (regex.test(numberString)) {
				numberString = numberString.replace(',', '.')
				const parsedValue = parseFloat(numberString)
				return isNaN(parsedValue) ? undefined : parsedValue
			}
			return undefined
		}

		const groupSeparator = formatter.format(11111).replace(/\p{Number}/gu, '')
		const decimalSeparator = formatter.format(1.1).replace(/\p{Number}/gu, '')

		// Nahradíme oddělovače skupin a desetinné tečky odpovídajícími znaky
		const sanitizedNumberString = numberString
			.split(groupSeparator)
			.join('')
			.replace(decimalSeparator, '.')
		// Pokud je číslo validní a odpovídá formátu, vrátíme ho
		if (regex.test(sanitizedNumberString)) {
			const parsedValue = parseFloat(sanitizedNumberString)
			return isNaN(parsedValue) ? undefined : parsedValue
		}

		console.error('Invalid number format')
		return undefined
	}
	function updateCursorPosition(distanceFromEnd: number) {
		const input = document.querySelector('.input input') as HTMLInputElement
		let newCursorPosition = () => {
			let position = input.value.length - distanceFromEnd
			if (position < 0) {
				position = 0
			}
			return position
		}
		input?.setSelectionRange(newCursorPosition(), newCursorPosition())
	}
</script>

{#snippet buttons()}
	<Button
		{dimension}
		{disabled}
		variant="secondary"
		onclick={() => {
			value -= 1
			stringValue = numberToLocalString(value, formatter)
		}}
	>
		<Subtract {size} />
	</Button>
	<Button
		{dimension}
		{disabled}
		variant="secondary"
		onclick={() => {
			value += 1
			stringValue = numberToLocalString(value, formatter)
		}}
	>
		<Add {size} />
	</Button>
{/snippet}

<Input
	class="input"
	value={stringValue}
	oninput={(e) => {
		stringValue = e.currentTarget.value
		const cursorPosition = e.currentTarget.selectionStart
		const distanceFromEnd = stringValue.length - (cursorPosition ?? 0)
		stringValue = stringValue.replace(/[^-?0-9.,]+/g, '').replace(/(?!^)-/g, '')
		const temp = localeStringToNumber(stringValue, formatter)
		if (temp === undefined) {
			const parts = stringValue.split('.')
			// If there are more than two parts, join only the first two and ignore the rest
			if (parts.length > 2) {
				stringValue = parts[0] + '.' + parts.slice(1).join('').replace(/\./g, '')
			}
			console.error('Not a number')
		} else {
			console.log('Valid number')
			value = temp
			stringValue = numberToLocalString(value, formatter)
		}
		setTimeout(() => {
			updateCursorPosition(distanceFromEnd)
		}, 0)
	}}
	{dimension}
	{disabled}
	{buttons}
	{...restProps}
/>
