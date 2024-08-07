<script lang="ts">
	import slider from '$lib/components/ui/slider.svelte?raw'
	import rangeSlider from '$lib/components/ui/range-slider.svelte?raw'
	import Code from '$lib/components/custom/code.svelte'
	import TabContent from '$lib/components/custom/tab-bar/tab-content.svelte'
	import { onMount } from 'svelte'
	import Typography from '$lib/components/ui/typography.svelte'
	import Select from '$lib/components/ui/select/select.svelte'
	import Option from '$lib/components/ui/select/option.svelte'
	import ComponentTemplate from '$lib/components/custom/component-template.svelte'
	import Input from '$lib/components/ui/input/input.svelte'
	import Slider from '$lib/components/ui/slider.svelte'
	import Switch from '$lib/components/ui/switch.svelte'
	import CodeComponentTemplate from '$lib/components/custom/code-component-template.svelte'
	import RangeSlider from '$lib/components/ui/range-slider.svelte'
	import NumberInput from '$lib/components/ui/input/number-input.svelte'

	type Layout = 'vertical' | 'horizontal'
	type Dimension = 'default' | 'large' | 'compact' | 'small'

	let sliderCss: string = $state('Loading...')
	let rangeSliderCss: string = $state('Loading...')

	let dimension: Dimension = $state('default')
	let label = $state('Slider label')
	let withStep: boolean = $state(false)
	let showSteps: boolean = $state(false)
	let step: number = $state(1)
	let centered: boolean = $state(false)
	let withHelperText: boolean = $state(false)
	let optionalHelperText: string = $state('Helper text')
	let layout: Layout = $state('vertical')
	let value = $state(30)
	let range = $state(false)
	let continuous: boolean = $state(false)

	// Svelte compiler breaks when it finds closing script tag, hence the need to make the template literal to have two parts
	let useCode = $derived(
		`<script lang="ts">${
			!range
				? `
	import Slider from '$lib/components/ui/slider.svelte'`
				: `
	import RangeSlider from '$lib/components/ui/range-slider.svelte'`
		}
</script` +
			`>${
				withHelperText
					? `
{#snippet helperText()}
    ${optionalHelperText}
{/snippet}`
					: ''
			}${
				!range
					? `
<Slider dimension="${dimension}" layout="${layout}"${withHelperText ? ` {helperText}` : ''}${withStep ? ` step={${step}}` : ''}${showSteps ? ` showSteps` : ''}${centered ? ` centered` : ''}>${label}</Slider>`
					: `
	<RangeSlider dimension="${dimension}" layout="${layout}"${withHelperText ? ` {helperText}` : ''}${withStep ? ` step={${step}}` : ''}${showSteps ? ` showSteps` : ''}>${label}</RangeSlider>`
			}

`,
	)

	onMount(async () => {
		const responseSlider = await fetch('/generated/css/ui/slider.css')
		sliderCss = await responseSlider.text()
		const responseRangeSlider = await fetch('/generated/css/ui/range-slider.css')
		rangeSliderCss = await responseRangeSlider.text()
	})
</script>

{#snippet description()}
	<Typography>
		Slider is available in 4 sizes: Default, Large, Compact and Small.
		<br />
		<br />
		Slider is also available in vertical and horizontal layout.
		<br />
		<br />
	</Typography>
{/snippet}

{#snippet examples()}
	<div class="example-row">
		<Typography variant="small" bold>1. Default size slider</Typography>
		<Slider dimension="default" bind:value>Slider label</Slider>
	</div>
	<div class="example-row">
		<Typography variant="small" bold>1. Large size slider</Typography>
		<Slider dimension="large" bind:value>Slider label</Slider>
	</div>
	<div class="example-row">
		<Typography variant="small" bold>1. Compact size slider</Typography>
		<Slider dimension="compact" bind:value>Slider label</Slider>
	</div>
	<div class="example-row">
		<Typography variant="small" bold>4. Small size slider</Typography>
		<Slider dimension="small" bind:value>Slider label</Slider>
	</div>
	<div class="example-row">
		<Typography variant="small" bold>1. Vertical layout</Typography>
		<Slider layout="vertical" bind:value>Slider label</Slider>
	</div>
	<div class="example-row">
		<Typography variant="small" bold>2. Horizontal layout</Typography>
		<Slider layout="horizontal" bind:value>Slider label</Slider>
	</div>
{/snippet}

{#snippet controls()}
	<Select bind:value={dimension} label="Slider size">
		<Option value="default">Default</Option>
		<Option value="large">Large</Option>
		<Option value="compact">Compact</Option>
		<Option value="small">Small</Option>
	</Select>
	<Select bind:value={layout} label="Slider layout">
		<Option value="vertical">Vertical</Option>
		<Option value="horizontal">Horizontal</Option>
	</Select>
	<Input bind:value={label} label="Slider label" />
	<Switch bind:checked={withHelperText} label="With helper text" />
	{#if withHelperText}
		<Input bind:value={optionalHelperText} label="Helper text" />
	{/if}
	<Switch bind:checked={withStep} label="With steps" />
	{#if withStep}
		<NumberInput bind:value={step} label="Step size" />
		<Switch bind:checked={showSteps} label="Show steps" />
	{/if}
	{#if !range}
		<Switch bind:checked={centered} label="Centered" />
	{/if}
	<Switch bind:checked={range} label="Range slider" />
	<Switch bind:checked={centered} label="Centered" />
	<Switch bind:checked={continuous} label="Continuous" />
{/snippet}

{#snippet helperText()}
	{optionalHelperText}
{/snippet}

{#snippet preview()}
	{#if range}
		<RangeSlider
			{dimension}
			{layout}
			helperText={withHelperText ? helperText : undefined}
			step={withStep ? step : undefined}
			{showSteps}
		>
			{label}
		</RangeSlider>
	{:else}
		<Slider
			{dimension}
			{layout}
			helperText={withHelperText ? helperText : undefined}
			step={withStep ? step : undefined}
			{showSteps}
			{centered}
			bind:value
		>
			{label}
		</Slider>
	{/if}
{/snippet}

{#snippet extraSvelte()}
	<TabContent value="slider"><Code language="svelte" code={slider} /></TabContent>
	<TabContent value="range-slider"><Code language="svelte" code={rangeSlider} /></TabContent>
{/snippet}

{#snippet extraCss()}
	<TabContent value="slider"><Code language="css" code={sliderCss} /></TabContent>
	<TabContent value="range-slider"><Code language="css" code={rangeSliderCss} /></TabContent>
{/snippet}

{#snippet implement()}
	<CodeComponentTemplate {extraSvelte} {extraCss} />
{/snippet}

<ComponentTemplate
	name="Slider"
	tagline="Sliders allow people to adjust a value within a specified range, with a single swipe or drag."
	{description}
	{examples}
	{controls}
	{preview}
	{useCode}
	{implement}
/>

<style lang="postcss">
	.example-row {
		display: flex;
		flex: 1;
		flex-wrap: wrap;
		align-items: center;
		gap: 16px;
		margin-top: 0px;
		margin-bottom: 0px;
	}
</style>
