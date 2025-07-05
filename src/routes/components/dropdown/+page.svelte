<script lang="ts">
	import dropdown from '$lib/components/ui/dropdown.svelte?raw'
	import Code from '$lib/components/custom/code.svelte'
	import TabContent from '$lib/components/custom/tab-bar/tab-content.svelte'
	import { onMount } from 'svelte'
	import Typography from '$lib/components/ui/typography.svelte'
	import Button from '$lib/components/ui/button.svelte'
	import Dropdown from '$lib/components/ui/dropdown.svelte'
	import {
		ChevronDown,
		OverflowMenuVertical,
		Settings,
		User,
		Edit,
		TrashCan,
		Add
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

	let variant: Variant = $state('ghost')
	let dimension: Dimension = $state('default')
	let withIcon: boolean = $state(true)
	let up: boolean = $state(false)
	let left: boolean = $state(false)
	let autoClose: boolean = $state(true)
	let withMode: boolean = $state(false)
	let mode: 'light' | 'dark' = $state('light')

	// Svelte compiler breaks when it finds closing script tag, hence the need to make the template literal to have two parts
	let useCode = $derived(
		`<script lang="ts">
import Dropdown from '$lib/components/ui/dropdown.svelte'
import Button from '$lib/components/ui/button.svelte'
${withIcon ? `import { ChevronDown, Settings, User, Edit } from 'carbon-icons-svelte'` : ''}
</script` +
			`>

<Dropdown buttonVariant="${variant}" buttonDimension="${dimension}"${up ? ` up` : ''}${left ? ` left` : ''}${!autoClose ? ` autoClose={false}` : ''}${withMode ? ` mode="${mode}"` : ''}>
	{#snippet button()}
		Options${withIcon ? `<ChevronDown size={16} />` : ''}
	{/snippet}
	{#snippet children()}
		<div class="dropdown-menu">
			<Button variant="ghost" dimension="compact">
				${withIcon ? `<Settings size={16} />` : ''}Settings
			</Button>
			<Button variant="ghost" dimension="compact">
				${withIcon ? `<User size={16} />` : ''}Profile
			</Button>
			<Button variant="ghost" dimension="compact">
				${withIcon ? `<Edit size={16} />` : ''}Edit
			</Button>
		</div>
	{/snippet}
</Dropdown>

<style>
	.dropdown-menu {
		display: flex;
		flex-direction: column;
		gap: 4px;
		padding: 8px;
		background: var(--colors-base);
		border: 1px solid var(--colors-low);
		border-radius: var(--border-radius);
	}
</style>
`,
	)

	onMount(async () => {
		const response = await fetch('/generated/css/ui/dropdown.css')
		if (response.ok) {
			css = await response.text()
		} else {
			css = '/* CSS not available */'
		}
	})
</script>

{#snippet description()}
	<Typography>
		Dropdown components provide a way to display a list of options or actions in a compact, 
		collapsible interface. They are triggered by clicking a button and can contain various types 
		of content including buttons, links, and other interactive elements.
		<br />
		<br />

		Dropdowns can be positioned relative to their trigger button using the `up` and `left` 
		properties to control placement. The `autoClose` property determines whether the dropdown 
		automatically closes when an item is selected.
		<br />
		<br />

		The component accepts custom button content through the `button` snippet and dropdown 
		content through the `children` snippet, providing flexibility in design and functionality.
	</Typography>
{/snippet}

{#snippet examples()}
	<p class="example-row">
		<Typography variant="small" bold>1. Basic dropdown</Typography>
		<Dropdown>
			{#snippet button()}
				Options <ChevronDown size={16} />
			{/snippet}
			{#snippet children()}
				<div class="dropdown-menu">
					<Button variant="ghost" dimension="compact">
						<Settings size={16} />Settings
					</Button>
					<Button variant="ghost" dimension="compact">
						<User size={16} />Profile
					</Button>
					<Button variant="ghost" dimension="compact">
						<Edit size={16} />Edit
					</Button>
				</div>
			{/snippet}
		</Dropdown>
	</p>

	<p class="example-row">
		<Typography variant="small" bold>2. Menu dropdown</Typography>
		<Dropdown buttonVariant="ghost">
			{#snippet button()}
				<OverflowMenuVertical size={20} />
			{/snippet}
			{#snippet children()}
				<div class="dropdown-menu">
					<Button variant="ghost" dimension="compact">
						<Add size={16} />Add item
					</Button>
					<Button variant="ghost" dimension="compact">
						<Edit size={16} />Edit
					</Button>
					<Button variant="ghost" dimension="compact">
						<TrashCan size={16} />Delete
					</Button>
				</div>
			{/snippet}
		</Dropdown>
	</p>

	<p class="example-row">
		<Typography variant="small" bold>3. Dropdown with different button variants</Typography>
		<Dropdown buttonVariant="strong">
			{#snippet button()}
				Actions <ChevronDown size={16} />
			{/snippet}
			{#snippet children()}
				<div class="dropdown-menu">
					<Button variant="ghost" dimension="compact">Action 1</Button>
					<Button variant="ghost" dimension="compact">Action 2</Button>
					<Button variant="ghost" dimension="compact">Action 3</Button>
				</div>
			{/snippet}
		</Dropdown>

		<Dropdown buttonVariant="secondary">
			{#snippet button()}
				More <ChevronDown size={16} />
			{/snippet}
			{#snippet children()}
				<div class="dropdown-menu">
					<Button variant="ghost" dimension="compact">Option A</Button>
					<Button variant="ghost" dimension="compact">Option B</Button>
					<Button variant="ghost" dimension="compact">Option C</Button>
				</div>
			{/snippet}
		</Dropdown>
	</p>

	<p class="example-row">
		<Typography variant="small" bold>4. Positioned dropdown (up)</Typography>
		<Dropdown up>
			{#snippet button()}
				Up dropdown <ChevronDown size={16} />
			{/snippet}
			{#snippet children()}
				<div class="dropdown-menu">
					<Button variant="ghost" dimension="compact">Opens upward</Button>
					<Button variant="ghost" dimension="compact">Item 2</Button>
				</div>
			{/snippet}
		</Dropdown>
	</p>

	<p class="example-row">
		<Typography variant="small" bold>5. Left-aligned dropdown</Typography>
		<Dropdown left>
			{#snippet button()}
				Left aligned <ChevronDown size={16} />
			{/snippet}
			{#snippet children()}
				<div class="dropdown-menu">
					<Button variant="ghost" dimension="compact">Left aligned</Button>
					<Button variant="ghost" dimension="compact">Item 2</Button>
				</div>
			{/snippet}
		</Dropdown>
	</p>
{/snippet}

{#snippet controls()}
	<Select bind:value={variant} label="Button variant">
		<Option value="strong">Strong</Option>
		<Option value="secondary">Secondary</Option>
		<Option value="ghost">Ghost</Option>
		<Option value="solid">Solid</Option>
		<Option value="darkoverlay">Dark overlay</Option>
		<Option value="lightoverlay">Light overlay</Option>
	</Select>

	<Select bind:value={dimension} label="Button size">
		<Option value="default">Default</Option>
		<Option value="large">Large</Option>
		<Option value="compact">Compact</Option>
		<Option value="small">Small</Option>
	</Select>

	<Switch label="With icon" bind:checked={withIcon}></Switch>
	<Switch label="Open upward" bind:checked={up}></Switch>
	<Switch label="Left aligned" bind:checked={left}></Switch>
	<Switch label="Auto close" bind:checked={autoClose}></Switch>
	<Switch label="With mode" bind:checked={withMode}></Switch>
	{#if withMode}
		<RadioGroup bind:value={mode}>
			<Radio value="light">Light</Radio>
			<Radio value="dark">Dark</Radio>
		</RadioGroup>
	{/if}
{/snippet}

{#snippet preview()}
	<Dropdown buttonVariant={variant} buttonDimension={dimension} {up} {left} {autoClose} mode={withMode ? mode : undefined}>
		{#snippet button()}
			Options
			{#if withIcon}
				<ChevronDown size={16} />
			{/if}
		{/snippet}
		{#snippet children()}
			<div class="dropdown-menu">
				<Button variant="ghost" dimension="compact">
					{#if withIcon}
						<Settings size={16} />
					{/if}
					Settings
				</Button>
				<Button variant="ghost" dimension="compact">
					{#if withIcon}
						<User size={16} />
					{/if}
					Profile
				</Button>
				<Button variant="ghost" dimension="compact">
					{#if withIcon}
						<Edit size={16} />
					{/if}
					Edit
				</Button>
			</div>
		{/snippet}
	</Dropdown>
{/snippet}

{#snippet extraSvelte()}
	<TabContent value="dropdown"><Code language="svelte" code={dropdown} /></TabContent>
{/snippet}

{#snippet extraCss()}
	<TabContent value="dropdown"><Code language="css" code={css} /></TabContent>
{/snippet}

{#snippet implement()}
	<CodeComponentTemplate {extraSvelte} {extraCss} />
{/snippet}

<ComponentTemplate
	name="Dropdown"
	tagline="Dropdowns display a list of options or actions in a compact, collapsible interface."
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

	.dropdown-menu {
		display: flex;
		flex-direction: column;
		gap: 4px;
		padding: 8px;
		background: var(--colors-base);
		border: 1px solid var(--colors-low);
		border-radius: var(--border-radius);
	}
</style>