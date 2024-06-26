<script lang="ts">
	import input from '$lib/components/ui/input/input.svelte?raw'
	import button from '$lib/components/ui/button.svelte?raw'
	import Code from '$lib/components/custom/code.svelte'
	import TabContent from '$lib/components/custom/tab-bar/tab-content.svelte'
	import { onMount } from 'svelte'
	import Select from '$lib/components/ui/select/select.svelte'
	import Option from '$lib/components/ui/select/option.svelte'
	import ComponentTemplate from '$lib/components/custom/component-template.svelte'
	import Input from '$lib/components/ui/input/input.svelte'
	import Switch from '$lib/components/ui/switch.svelte'
	import Checkbox from '$lib/components/ui/checkbox.svelte'
	import { ColorPalette, ArrowLeft } from 'carbon-icons-svelte'
	import Button from '$lib/components/ui/button.svelte'
	import Typography from '$lib/components/ui/typography.svelte'
	import CodeComponentTemplate from '$lib/components/custom/code-component-template.svelte'

	type Dimension = 'default' | 'large' | 'compact' | 'small'
	type Layout = 'vertical' | 'horizontal'
	type Variant = 'outline' | 'solid'

	let inputCss: string = $state('Loading...')
	let buttonCss: string = $state('Loading...')

	let dimension: Dimension = $state('default' as Dimension)
	let layout: Layout = $state('vertical')
	let label = $state('Input label')
	let placeholder = $state('Placeholder')
	let unit = $state(false)
	let stringUnit = $state('Kg')
	let errorMessage = $state(false)
	let stringErrorMessage = $state('Error')
	let controlButton = $state(false)
	let colorButton = $state(false)
	let arrowButton = $state(false)
	let withHelperText: boolean = $state(false)
	let optionalHelperText: string = $state('Helper text')
	let variant: Variant = $state('outline')

	// Svelte compiler breaks when it finds closing script tag, hence the need to make the template literal to have two parts
	let useCode = $derived(
		`<script lang="ts">
	import Input from '$lib/components/ui/input/input.svelte'${
		colorButton || arrowButton
			? `
	import Button from '$lib/components/ui/button.svelte'${
		colorButton && arrowButton
			? `
	import { ColorPalette, ArrowLeft } from 'carbon-icons-svelte' `
			: arrowButton
				? `
	import { ArrowLeft } from 'carbon-icons-svelte'`
				: colorButton
					? `
	import { ColorPalette } from 'carbon-icons-svelte'`
					: ''
	}`
			: ''
	}
</script` +
			`>${
				errorMessage
					? `
{#snippet error()}
	${stringErrorMessage}
{/snippet}`
					: ''
			}${
				controlButton
					? `
{#snippet buttons()}${
							colorButton
								? `
	<Button dimension="${dimension}" variant="secondary">
		<ColorPalette size=${dimension === `small` ? '{16}' : '{24}'} />
	</Button>`
								: ''
						}${
							arrowButton
								? `
	<Button dimension="${dimension}" variant="secondary">
		<ArrowLeft size=${dimension === `small` ? '{16}' : '{24}'} />
	</Button>`
								: ''
						}
{/snippet}`
					: ''
			}
<Input 
	dimension="${dimension}" 
	layout="${layout}"
	variant="${variant}" 
	label="${label}" 
	placeholder="${placeholder}"${
		unit
			? `
	unit="${stringUnit}"`
			: ''
	}${errorMessage ? '{error}' : ''}${
		controlButton
			? `
	controls
	{buttons} `
			: ''
	}${
		withHelperText
			? `>
	${optionalHelperText}
</Input>`
			: '/>'
	}
`,
	)

	onMount(async () => {
		const responseInput = await fetch('/generated/css/ui/input.css')
		inputCss = await responseInput.text()
		const responseButton = await fetch('/generated/css/ui/button.css')
		buttonCss = await responseButton.text()
	})
</script>

{#snippet description()}
	<Typography>Each input is available in 4 sizes: Default, Large, Compact and Small</Typography>
	<br />
	<br />
	<Typography>Input is available in vertical and horizontal layout</Typography>
	<br />
	<br />
{/snippet}
{#snippet examples()}
	<div class="example-row">
		<Typography variant="small" bold>1. Default size input</Typography>
		<Input dimension="default" label="Input label" />
	</div>
	<div class="example-row">
		<Typography variant="small" bold>2. Large size input</Typography>
		<Input dimension="large" label="Input label" />
	</div>
	<div class="example-row">
		<Typography variant="small" bold>3. Compact size input</Typography>
		<Input dimension="compact" label="Input label" />
	</div>
	<div class="example-row">
		<Typography variant="small" bold>4. Small size input</Typography>
		<Input dimension="small" label="Input label" />
	</div>
	<div class="example-row">
		<Typography variant="small" bold>1. Vertical layout</Typography>
		<Input layout="vertical" label="Input label" />
	</div>
	<div class="example-row">
		<Typography variant="small" bold>2. Horizontal layout</Typography>
		<Input layout="horizontal" label="Input label" />
	</div>
{/snippet}

{#snippet controls()}
	<Select bind:value={dimension} label="Size">
		<Option value="default">Default</Option>
		<Option value="large">Large</Option>
		<Option value="compact">Compact</Option>
		<Option value="small">Small</Option>
	</Select>
	<Select bind:value={layout} label="Layout">
		<Option value="vertical">Vertical</Option>
		<Option value="horizontal">Horizontal</Option>
	</Select>
	<Select bind:value={variant} label="Variant">
		<Option value="outline">Outline</Option>
		<Option value="solid">Solid</Option>
	</Select>
	<Input bind:value={label} {label} />
	<Input bind:value={placeholder} label="Placeholder" />
	<Switch bind:checked={withHelperText} label="With helper text" />
	{#if withHelperText}
		<Input label="Add helper text" bind:value={optionalHelperText} />
	{/if}
	<Switch bind:checked={unit} label="With unit" />
	{#if unit}
		<Input label="Add unit" bind:value={stringUnit} />
	{/if}
	<Switch bind:checked={errorMessage} label="With error" />
	{#if errorMessage}
		<Input label="Add error" bind:value={stringErrorMessage} />
	{/if}
	<Switch bind:checked={controlButton} label="With controls" />
	{#if controlButton}
		<Checkbox bind:checked={colorButton} label="Color palette button" />
		<Checkbox bind:checked={arrowButton} label="Arrow left button" />
	{/if}
{/snippet}

{#snippet error()}
	{stringErrorMessage}
{/snippet}
{#snippet buttons()}
	{#if colorButton}
		<Button {dimension} variant="secondary">
			<ColorPalette size={dimension === 'small' ? 16 : 24} />
		</Button>
	{/if}
	{#if arrowButton}
		<Button {dimension} variant="secondary">
			<ArrowLeft size={dimension === 'small' ? 16 : 24} />
		</Button>
	{/if}
{/snippet}

{#snippet preview()}
	<Input
		{dimension}
		{layout}
		{variant}
		{label}
		{placeholder}
		unit={unit ? stringUnit : ''}
		error={errorMessage ? error : undefined}
		buttons={controlButton ? buttons : undefined}
	>
		{#if withHelperText}
			{optionalHelperText}
		{/if}</Input
	>
{/snippet}

{#snippet extraSvelte()}
	<TabContent value="input"><Code language="svelte" code={input} /></TabContent>
	<TabContent value="button"><Code language="svelte" code={button} /></TabContent>
{/snippet}

{#snippet extraCss()}
	<TabContent value="input"><Code language="css" code={inputCss} /></TabContent>
	<TabContent value="button"><Code language="css" code={buttonCss} /></TabContent>
{/snippet}

{#snippet implement()}
	<CodeComponentTemplate {extraSvelte} {extraCss} />
{/snippet}

<ComponentTemplate
	name="Input"
	tagline="Input elements allow people to enter and edit text or data within a form."
	{description}
	{examples}
	{controls}
	{preview}
	{useCode}
	{implement}
/>
