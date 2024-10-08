<script lang="ts">
	import button from '$lib/components/ui/button.svelte?raw'
	import Code from '$lib/components/custom/code.svelte'
	import TabContent from '$lib/components/custom/tab-bar/tab-content.svelte'
	import { onMount } from 'svelte'
	import Typography from '$lib/components/ui/typography.svelte'
	import Button from '$lib/components/ui/button.svelte'
	import {
		Checkmark,
		ArrowRight,
		Close,
		Information,
		ChevronRight,
		Save,
		OverflowMenuVertical,
		Launch,
		CaretRight,
		TrashCan,
		SidePanelOpen,
	} from 'carbon-icons-svelte'
	import Switch from '$lib/components/ui/switch.svelte'
	import Select from '$lib/components/ui/select/select.svelte'
	import Option from '$lib/components/ui/select/option.svelte'
	import ComponentTemplate from '$lib/components/custom/component-template.svelte'
	import CodeComponentTemplate from '$lib/components/custom/code-component-template.svelte'
	import RadioGroup from '$lib/components/ui/radio-button/radio-group.svelte'
	import Radio from '$lib/components/ui/radio-button/radio.svelte'

	type Variant = 'strong' | 'secondary' | 'ghost' | 'solid' | 'darkoverlay' | 'lightoverlay'
	type Dimension = 'default' | 'large' | 'compact' | 'small'

	let css: string = $state('Loading...')

	let variant: Variant = $state('strong')
	let dimension: Dimension = $state('default')
	let leftIcon: boolean = $state(true)
	let rightIcon: boolean = $state(false)
	let withMode: boolean = $state(false)
	let mode: 'light' | 'dark' = $state('light')

	// Svelte compiler breaks when it finds closing script tag, hence the need to make the template literal to have two parts
	let useCode = $derived(
		`<script lang="ts">
import Button from '$lib/components/ui/button.svelte'
${leftIcon || rightIcon ? `import { Close } from 'carbon-icons-svelte'` : ''}
</script` +
			`>

<Button variant="${variant}" dimension="${dimension}"${withMode ? ` mode="${mode}` : ''}>
	${leftIcon ? `<Close/>` : ''}Close${rightIcon ? `<Close/>` : ''}
</Button>
`,
	)

	onMount(async () => {
		const response = await fetch('/generated/css/ui/button.css')
		css = await response.text()
	})
</script>

{#snippet description()}
	<Typography>
		There are 6 types of buttons available.
		<br />
		<br />

		Each button is available in 4 sizes: Default, Large, Compact and Small.
		<br />
		<br />

		Different button types and sizes can be combined on the same screen to create hierarchy and
		direct attention to the most important actions.

		<br />
		<br />

		Solid and overlay buttons should be used to display a button floating over content (image or
		text for example).
	</Typography>
{/snippet}

{#snippet examples()}
	<p class="example-row">
		<Typography variant="small" bold>1. Strong button</Typography>
		<Button variant="strong">Strong button</Button>
		<Button variant="strong"><Checkmark size={24} />Confirm</Button>
		<Button variant="strong">Proceed<ArrowRight size={24} /></Button>
	</p>

	<p class="example-row">
		<Typography variant="small" bold>2. Outline button</Typography>
		<Button variant="secondary">Button</Button>
		<Button variant="secondary"><Close size={24} />Cancel</Button>
		<Button variant="secondary">Next<ArrowRight size={24} /></Button>
	</p>

	<p class="example-row">
		<Typography variant="small" bold>3. Ghost button</Typography>
		<Button variant="ghost">Ghost button</Button>
		<Button variant="ghost"><Information size={24} />Info</Button>
		<Button variant="ghost">More<ChevronRight size={24} /></Button>
	</p>

	<p class="example-row">
		<Typography variant="small" bold>4. Strong icon button</Typography>
		<Button variant="strong"><Save size={24} /></Button>
	</p>

	<p class="example-row">
		<Typography variant="small" bold>5. Icon button</Typography>
		<Button variant="ghost"><OverflowMenuVertical size={24} /></Button>
	</p>

	<p class="example-row">
		<Typography variant="small" bold>6. Dark overlay button</Typography>
		<Button variant="darkoverlay">Overlay</Button>
		<Button variant="darkoverlay"><Checkmark size={24} />Done</Button>
		<Button variant="darkoverlay">Next<ArrowRight size={24} /></Button>
	</p>

	<p class="example-row">
		<Typography variant="small" bold>7. Light overlay button</Typography>
		<Button variant="lightoverlay">Overlay</Button>
		<Button variant="lightoverlay"><Checkmark size={24} />Done</Button>
		<Button variant="lightoverlay">Next<ArrowRight size={24} /></Button>
	</p>

	<p class="example-row">
		<Typography variant="small" bold>8. Solid button</Typography>
		<Button variant="solid">Solid</Button>
		<Button variant="solid"><Launch size={24} />Open</Button>
		<Button variant="solid">See all<CaretRight size={24} /></Button>
	</p>

	<p class="example-row">
		<Typography variant="small" bold>9. Dark overlay icon button</Typography>
		<Button variant="darkoverlay"><TrashCan size={24} /></Button>
	</p>

	<p class="example-row">
		<Typography variant="small" bold>10. Light overlay icon button</Typography>
		<Button variant="lightoverlay"><TrashCan size={24} /></Button>
	</p>

	<p class="example-row">
		<Typography variant="small" bold>11. Solid icon button</Typography>
		<Button variant="solid"><SidePanelOpen size={24} /></Button>
	</p>
{/snippet}

{#snippet helperTextButtonType()}
	More info on button types: <a href="#choose-a-button-type">Choose a button type</a>
{/snippet}

{#snippet controls()}
	<Select bind:value={variant} label="Button type" helperText={helperTextButtonType}>
		<Option value="strong">Strong button</Option>
		<Option value="secondary">Outline button</Option>
		<Option value="ghost">Ghost button</Option>
		<Option value="solid">Solid button</Option>
		<Option value="darkoverlay">Dark overlay button</Option>
		<Option value="lightoverlay">Light overlay button</Option>
	</Select>

	<Select bind:value={dimension} label="Button size">
		<Option value="default">Default</Option>
		<Option value="large">Large</Option>
		<Option value="compact">Compact</Option>
		<Option value="small">Small</Option>
	</Select>

	<Switch label="Left icon" bind:checked={leftIcon}></Switch>
	<Switch label="Right icon" bind:checked={rightIcon}></Switch>
	<Switch label="With mode" bind:checked={withMode}></Switch>
	{#if withMode}
		<RadioGroup bind:value={mode}>
			<Radio value="light">Light</Radio>
			<Radio value="dark">Dark</Radio>
		</RadioGroup>
	{/if}
{/snippet}

{#snippet preview()}
	<Button {variant} {dimension} mode={withMode ? mode : undefined}>
		{#if leftIcon}
			<Close size={24} />
		{/if}
		Close
		{#if rightIcon}
			<Close size={24} />
		{/if}
	</Button>
{/snippet}

{#snippet extraSvelte()}
	<TabContent value="button"><Code language="svelte" code={button} /></TabContent>
{/snippet}

{#snippet extraCss()}
	<TabContent value="button"><Code language="css" code={css} /></TabContent>
{/snippet}

{#snippet implement()}
	<CodeComponentTemplate {extraSvelte} {extraCss} />
{/snippet}

<ComponentTemplate
	name="Button"
	tagline="Buttons allow people to perform an instantaneous action, with a single tap/click."
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
