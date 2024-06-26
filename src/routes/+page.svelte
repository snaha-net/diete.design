<script lang="ts">
	import {
		Checkmark,
		ArrowRight,
		Settings,
		Fire,
		NotificationNew,
		ThumbsUp,
		Star,
		CheckmarkFilled,
		WarningAltFilled,
		GameWireless,
		Basketball,
		Analytics,
		Encryption,
		MachineLearning,
	} from 'carbon-icons-svelte'

	import Button from '$lib/components/ui/button.svelte'
	import Checkbox from '$lib/components/ui/checkbox.svelte'
	import Input from '$lib/components/ui/input/input.svelte'
	import Radio from '$lib/components/ui/radio-button/radio.svelte'
	import Typography from '$lib/components/ui/typography.svelte'
	import Switch from '$lib/components/ui/switch.svelte'
	import Select from '$lib/components/ui/select/select.svelte'
	import Option from '$lib/components/ui/select/option.svelte'
	import RadioGroup from '$lib/components/ui/radio-button/radio-group.svelte'
	import Slider from '$lib/components/ui/slider.svelte'
	import ResponsiveContainer from '$lib/components/custom/responsive-container.svelte'
	import type { Dimension } from '$lib/components/ui/menu/menu-store.svelte'
	import Code from '$lib/components/custom/code.svelte'
	import Badge from '$lib/components/ui/badge.svelte'
	import MenuTitle from '$lib/components/ui/menu/menu-title.svelte'
	import MenuItem from '$lib/components/ui/menu/menu-item.svelte'
	import RangeSlider from '$lib/components/ui/range-slider.svelte'
	import Textarea from '$lib/components/ui/textarea.svelte'
	import Divider from '$lib/components/ui/divider.svelte'
	import SearchInput from '$lib/components/ui/input/search-input.svelte'
	import DateInput from '$lib/components/ui/input/date-input.svelte'
	import ColorInput from '$lib/components/ui/input/color-input.svelte'

	let dimension: Dimension = $state('default')
	let size: 16 | 24 | 32 = $derived(
		dimension === 'default' || dimension === 'compact'
			? 24
			: dimension === 'large'
				? 32
				: dimension === 'small'
					? 16
					: 24,
	)
</script>

<Typography variant="h1" bold
	>Open-source, customisable & fully accessible design system – built with Svelte</Typography
>

<section id="introduction">
	<Typography variant="large">
		Diète is made by, and for, free-spirited app makers who want to remain independent. All
		components are minimally-designed, built with Svelte 5, easily customisable and comply with the
		highest accessibility standards. Check our growing library of open-source UI components, that
		you can copy directly into your project.
	</Typography>
</section>

<section id="call-to-action">
	<Button variant="strong" dimension="large" href="/components/button">Explore components</Button>
	<Button variant="secondary" dimension="large" href="/build">Learn more</Button>
</section>

<Divider />

<ResponsiveContainer class="vspace">
	<section id="preview-title">
		<Typography variant="h4">Quick preview</Typography>
	</section>
	<section id="size-select">
		<Select layout="horizontal" label="Size" bind:value={dimension}>
			<Option value="default">Default</Option>
			<Option value="large">Large</Option>
			<Option value="compact">Compact</Option>
			<Option value="small">Small</Option>
		</Select>
	</section>
</ResponsiveContainer>

<ResponsiveContainer class="vspace">
	<section id="strong-buttons" class="controls buttons">
		<Button variant="strong" {dimension}>Strong button</Button>
		<Button variant="strong" {dimension}><Checkmark {size} /> Strong button</Button>
		<Button variant="strong" {dimension}>Strong button <ArrowRight {size} /></Button>
		<Button variant="strong" {dimension}><Settings {size} /></Button>
	</section>

	<section id="outline-buttons" class="controls buttons">
		<Button variant="secondary" {dimension}>Outline button</Button>
		<Button variant="secondary" {dimension}><Checkmark {size} /> Outline button</Button>
		<Button variant="secondary" {dimension}>Outline button <ArrowRight {size} /></Button>
		<Button variant="secondary" {dimension}><Settings {size} /></Button>
	</section>

	<section id="solid-buttons" class="controls buttons solid">
		<Button variant="solid" {dimension}>Solid button</Button>
		<Button variant="solid" {dimension}><Checkmark {size} /> Solid button</Button>
		<Button variant="solid" {dimension}>Solid button <ArrowRight {size} /></Button>
		<Button variant="solid" {dimension}><Settings {size} /></Button>
	</section>

	<section id="ghost-buttons" class="controls buttons">
		<Button variant="ghost" {dimension}>Ghost button</Button>
		<Button variant="ghost" {dimension}><Checkmark {size} /> Ghost button</Button>
		<Button variant="ghost" {dimension}>Ghost button <ArrowRight {size} /></Button>
		<Button variant="ghost" {dimension}><Settings {size} /></Button>
	</section>

	<section id="dark-overlay" class="controls buttons">
		<Button variant="darkoverlay" {dimension}>Dark overlay</Button>
		<Button variant="darkoverlay" {dimension}><Checkmark {size} /> Dark overlay</Button>
		<Button variant="darkoverlay" {dimension}>Dark overlay <ArrowRight {size} /></Button>
		<Button variant="darkoverlay" {dimension}><Settings {size} /></Button>
	</section>

	<section id="light-overlay" class="controls buttons">
		<Button variant="lightoverlay" {dimension}>Light overlay</Button>
		<Button variant="lightoverlay" {dimension}><Checkmark {size} /> Light overlay</Button>
		<Button variant="lightoverlay" {dimension}>Light overlay <ArrowRight {size} /></Button>
		<Button variant="lightoverlay" {dimension}><Settings {size} /></Button>
	</section>

	<section id="checkboxes" class="controls no-gap">
		<Checkbox {dimension} label="Succulents" checked={true} />
		<Checkbox {dimension} label="Tattooed fingerstache" checked={false} />
		<Checkbox {dimension} label="Asymmetrical" checked={true} />
		<Checkbox {dimension} label="Microdosing sriracha" checked={false} />
	</section>

	<section id="radio" class="controls">
		<RadioGroup value="1" {dimension}>
			<Radio value="1">Humblebrag</Radio>
			<Radio value="2">Solarpunk shaman</Radio>
			<Radio value="3">Single-origin</Radio>
			<Radio value="4">Moonbathing</Radio>
		</RadioGroup>
	</section>

	<section id="switches" class="controls no-gap">
		<Switch {dimension} label="Vape cleanse" checked={true} />
		<Switch {dimension} label="Moustache" checked={false} />
		<Switch {dimension} label="Farm-to-table" checked={true} />
		<Switch {dimension} label="Flexitarian mixtape" checked={false} />
	</section>
</ResponsiveContainer>

<ResponsiveContainer class="vspace">
	<section id="sliders" class="controls">
		<Slider layout="vertical" {dimension} alwaysShowValue>
			Continuous slider
			{#snippet helperText()}
				A simple continuous slider, showing the selected value on the right
			{/snippet}
		</Slider>

		<Divider />

		<Slider layout="vertical" {dimension}
			>Continuous slider with min/max
			{#snippet helperText()}
				Selected value is shown on hover, pressed and focused states
			{/snippet}
		</Slider>

		<Divider />

		<Slider layout="vertical" {dimension} step={10} showSteps alwaysShowValue
			>Discrete slider
			{#snippet helperText()}
				Selects a value from a range of predetermined values, marked by stop indicators
			{/snippet}
		</Slider>

		<Divider />

		<Slider layout="vertical" {dimension} step={10} showSteps
			>Discrete slider with min/max
			{#snippet helperText()}
				Discrete slider can also show minimum and maximum values
			{/snippet}
		</Slider>
	</section>

	<section id="centered-sliders" class="controls">
		<Slider layout="vertical" {dimension} centered min={-5} max={5} alwaysShowValue>
			Continuous centered slider
			{#snippet helperText()}
				Selects a value from a negative/positive range, starting value is in the center
			{/snippet}
		</Slider>

		<Divider />

		<Slider layout="vertical" {dimension} centered min={-5} max={5}
			>Continuous centered slider with min/max
			{#snippet helperText()}
				Selected value is shown on hover, pressed and focused states
			{/snippet}
		</Slider>

		<Divider />

		<Slider
			layout="vertical"
			{dimension}
			step={1}
			showSteps
			centered
			min={-5}
			max={5}
			alwaysShowValue
			>Discrete centered slider
			{#snippet helperText()}
				Selects a value from a range of predetermined values, marked by stop indicators
			{/snippet}
		</Slider>

		<Divider />

		<Slider layout="vertical" {dimension} step={1} showSteps centered min={-5} max={5}
			>Discrete slider with min/max
			{#snippet helperText()}
				Discrete slider can also show minimum and maximum values
			{/snippet}
		</Slider>
	</section>
</ResponsiveContainer>

<ResponsiveContainer class="vspace">
	<section class="controls">
		<RangeSlider layout="vertical" {dimension}>
			Continuous range slider
			{#snippet helperText()}
				Selects two values on one slider to create a range
			{/snippet}
		</RangeSlider>

		<Divider />

		<RangeSlider layout="vertical" {dimension} step={10} showSteps>
			Discrete slider with min/max
			{#snippet helperText()}
				Range slider is also available with discrete values
			{/snippet}
		</RangeSlider>
	</section>
</ResponsiveContainer>

<ResponsiveContainer class="vspace">
	<section id="search" class="controls">
		<SearchInput {dimension} placeholder="Search" />
		<SearchInput {dimension} placeholder="Search" withButton={true} />
	</section>

	<section id="search-solid" class="controls solid">
		<SearchInput {dimension} variant="solid" placeholder="Search" />
		<SearchInput {dimension} variant="solid" placeholder="Search" withButton={true} />
	</section>
</ResponsiveContainer>

<ResponsiveContainer class="vspace">
	<section id="input" class="controls">
		<Input {dimension} value="Williamsburg" label="Text input">This is some helper text</Input>

		<Divider />

		<Input
			{dimension}
			type="number"
			min="0"
			max="69420"
			step="0.001"
			value="1.420"
			label="Number input"
			unit="EUR">This is some helper text</Input
		>
		<Divider />

		<DateInput {dimension} value="2024-05-01" label="Date input">This is some helper text</DateInput
		>

		<Divider />

		<ColorInput {dimension} label="Color input">This is some helper text</ColorInput>
	</section>

	<section id="input-solid" class="controls solid">
		<Input {dimension} variant="solid" value="Williamsburg" label="Text input"
			>This is some helper text</Input
		>

		<Divider />

		<Input
			{dimension}
			variant="solid"
			type="number"
			min="0"
			max="69420"
			step="0.001"
			value="1.420"
			label="Number input"
			unit="EUR">This is some helper text</Input
		>

		<Divider />

		<DateInput {dimension} variant="solid" value="2024-05-01" label="Date input"
			>This is some helper text</DateInput
		>

		<Divider />

		<ColorInput {dimension} variant="solid" label="Color input">This is some helper text</ColorInput
		>
	</section>
</ResponsiveContainer>

{#snippet helperText()}
	This is an optional helper text
{/snippet}

<ResponsiveContainer class="vspace">
	<section id="textarea" class="controls">
		<Textarea
			label="Text area"
			{helperText}
			{dimension}
			value="I'm baby glossier artisan cred austin, yuccie venmo aesthetic seitan franzen pour-over taiyaki."
		></Textarea>
	</section>
	<section id="textarea-solid" class="controls solid">
		<Textarea
			label="Text area"
			{helperText}
			{dimension}
			value="I'm baby glossier artisan cred austin, yuccie venmo aesthetic seitan franzen pour-over taiyaki."
			variant="solid"
		></Textarea>
	</section>
</ResponsiveContainer>

<ResponsiveContainer class="vspace">
	<section id="select" class="controls">
		<Select value="1" label="Select">
			{#snippet helperText()}
				This is some helper text
			{/snippet}
			<Option value="1">Banjo tacos</Option>
			<Option value="2">Pickled chicharrones</Option>
			<Option value="3">Vinegar vape</Option>
			<Option value="4">Denim brunch vinyl</Option>
		</Select>
	</section>

	<section id="select-solid" class="controls solid">
		<Select value="1" label="Select" variant="solid">
			{#snippet helperText()}
				This is some helper text
			{/snippet}
			<Option value="1">Banjo tacos</Option>
			<Option value="2">Pickled chicharrones</Option>
			<Option value="3">Vinegar vape</Option>
			<Option value="4">Denim brunch vinyl</Option>
		</Select>
	</section>
</ResponsiveContainer>

<ResponsiveContainer class="vspace">
	<section class="controls">
		<MenuTitle bold open>
			{#snippet content()}
				Topics
			{/snippet}
			<MenuItem>
				<div class="row space-between grow">
					<div class="row">
						<GameWireless size={24} />
						Gaming
					</div>
					<Badge>6</Badge>
				</div>
			</MenuItem>
			<MenuItem>
				<div class="row space-between grow">
					<div class="row">
						<Basketball size={24} />
						Sports
					</div>
					<Badge>12</Badge>
				</div>
			</MenuItem>
			<MenuItem>
				<div class="row space-between grow">
					<div class="row">
						<Analytics size={24} />
						Business
					</div>
					<Badge>8</Badge>
				</div>
			</MenuItem>
			<MenuItem>
				<div class="row space-between grow">
					<div class="row">
						<Encryption size={24} />
						Crypto
					</div>
					<Badge>24</Badge>
				</div>
			</MenuItem>
			<MenuItem>
				<div class="row space-between grow">
					<div class="row">
						<Fire size={24} />
						Trending
					</div>
					<Badge>420</Badge>
				</div>
			</MenuItem>
		</MenuTitle>
	</section>

	<section class="controls">
		<MenuTitle bold open>
			{#snippet content()}
				<MachineLearning size={24} />
				Letterpress intelligentsia
			{/snippet}
			<MenuItem>
				<div class="row space-between grow">
					Enamel pin
					<ArrowRight size={24} />
				</div>
			</MenuItem>
			<MenuItem>
				<div class="row space-between grow">
					Cold-pressed hammock
					<ArrowRight size={24} />
				</div>
			</MenuItem>
			<MenuItem>
				<div class="row space-between grow">
					Polaroid shaman
					<ArrowRight size={24} />
				</div>
			</MenuItem>
			<MenuItem>
				<div class="row space-between grow">
					Sriracha master
					<ArrowRight size={24} />
				</div>
			</MenuItem>
			<MenuItem>
				<div class="row space-between grow">
					Thundercats
					<ArrowRight size={24} />
				</div>
			</MenuItem>
		</MenuTitle>
	</section>

	<section class="controls">
		<MenuTitle open>
			{#snippet content()}
				File
			{/snippet}
			<MenuItem>
				<div class="row space-between grow">
					Edit
					<span class="opacity"> Ctrl+E </span>
				</div>
			</MenuItem>
			<MenuItem>
				<div class="row space-between grow">
					Copy
					<span class="opacity"> Ctrl+C </span>
				</div>
			</MenuItem>
			<MenuItem>
				<div class="row space-between grow">
					Find
					<span class="opacity"> Ctrl+F </span>
				</div>
			</MenuItem>
			<MenuItem>Emojis & symbols</MenuItem>
			<MenuItem>
				<div class="row space-between grow">
					Speech
					<span class="opacity"> Ctrl+P </span>
				</div>
			</MenuItem>
		</MenuTitle>
	</section>
</ResponsiveContainer>

<ResponsiveContainer class="vspace">
	<section id="text-sans" class="controls">
		<Typography font="sans" variant="h1">H1 title</Typography>
		<Typography font="sans" variant="h2">H2 title</Typography>
		<Typography font="sans" variant="h3">H3 title</Typography>
		<Typography font="sans" variant="h4">H4 title</Typography>
		<Typography font="sans" variant="large">Large paragraph</Typography>
		<Typography font="sans" variant="large"><a href="#/">Large text link</a></Typography>
		<Typography font="sans" variant="h5">H5 title</Typography>
		<Typography font="sans" variant="default">Paragraph</Typography>
		<Typography font="sans" variant="default"><a href="#/">Text link</a></Typography>
		<Typography font="sans" variant="h6">H6 title</Typography>
		<Typography font="sans" variant="small">Small paragraph</Typography>
		<Typography font="sans" variant="small"><a href="#/">Small text link</a></Typography>
	</section>

	<section id="text-serif" class="controls">
		<Typography font="serif" variant="h1">H1 title</Typography>
		<Typography font="serif" variant="h2">H2 title</Typography>
		<Typography font="serif" variant="h3">H3 title</Typography>
		<Typography font="serif" variant="h4">H4 title</Typography>
		<Typography font="serif" variant="large">Large paragraph</Typography>
		<Typography font="serif" variant="large"><a href="#/">Large text link</a></Typography>
		<Typography font="serif" variant="h5">H5 title</Typography>
		<Typography font="serif" variant="default">Paragraph</Typography>
		<Typography font="serif" variant="default"><a href="#/">Text link</a></Typography>
		<Typography font="serif" variant="h6">H6 title</Typography>
		<Typography font="serif" variant="small">Small paragraph</Typography>
		<Typography font="serif" variant="small"><a href="#/">Small text link</a></Typography>
	</section>

	<section id="text-mono" class="controls">
		<Typography font="mono" variant="h1">H1 title</Typography>
		<Typography font="mono" variant="h2">H2 title</Typography>
		<Typography font="mono" variant="h3">H3 title</Typography>
		<Typography font="mono" variant="h4">H4 title</Typography>
		<Typography font="mono" variant="large">Large paragraph</Typography>
		<Typography font="mono" variant="large"><a href="#/">Large text link</a></Typography>
		<Typography font="mono" variant="h5">H5 title</Typography>
		<Typography font="mono" variant="default">Paragraph</Typography>
		<Typography font="mono" variant="default"><a href="#/">Text link</a></Typography>
		<Typography font="mono" variant="h6">H6 title</Typography>
		<Typography font="mono" variant="small">Small paragraph</Typography>
		<Typography font="mono" variant="small"><a href="#/">Small text link</a></Typography>
	</section>
</ResponsiveContainer>

<ResponsiveContainer class="vspace">
	<section id="badge" class="controls no-gap">
		<div class="row">
			<Typography>Skatebord</Typography>
			<Badge dimension="small">20% off</Badge>
		</div>

		<Divider class="half-margin" />

		<div class="row space-between">
			<Typography variant="h5">Teriyaki chicken</Typography>
			<Badge dimension="small">
				<Fire size={16} />
				Spicy
			</Badge>
		</div>

		<Divider class="half-margin" />

		<div class="row">
			<Typography variant="large">KGLW</Typography>
			<Badge dimension="small" variant="strong">NEW RELEASE</Badge>
		</div>

		<Divider class="half-margin" />

		<div class="row space-between">
			<Typography variant="h5">Messages</Typography>
			<Badge dimension="small" variant="strong">
				<NotificationNew size={16} />
				420
			</Badge>
		</div>

		<Divider class="half-margin" />

		<div class="row space-between">
			<Typography variant="h3">Yuccie</Typography>
			<Badge dimension="large">On sale</Badge>
		</div>

		<Divider class="half-margin" />

		<div class="row">
			<Typography variant="large">Sriracha</Typography>
			<Badge dimension="large">
				<ThumbsUp size={24} />
				2,288
			</Badge>
		</div>

		<Divider class="half-margin" />

		<div class="row">
			<Typography variant="h3">Trust fund</Typography>
			<Badge dimension="large" variant="strong">NEW!</Badge>
		</div>

		<Divider class="half-margin" />

		<div class="row space-between">
			<Typography variant="large">Kale chips</Typography>
			<Badge dimension="large">
				<Star size={24} />
				17
			</Badge>
		</div>
	</section>

	<section id="dark-overlay" class="controls">
		<div class="col">
			<div class="row space-between">
				<Badge dimension="small" variant="dark-overlay">Trending</Badge>
				<Badge dimension="small" variant="dark-overlay">
					<CheckmarkFilled size={16} />
					Latest version
				</Badge>
			</div>
			<div class="row space-between">
				<Badge dimension="large" variant="dark-overlay">
					<WarningAltFilled size={24} />
					Error
				</Badge>
				<Badge dimension="large" variant="dark-overlay">Category</Badge>
			</div>
		</div>
	</section>

	<section id="light-overlay" class="controls">
		<div class="col">
			<div class="row space-between">
				<Badge dimension="small" variant="light-overlay">Trending</Badge>
				<Badge dimension="small" variant="light-overlay">
					<CheckmarkFilled size={16} />
					Latest version
				</Badge>
			</div>
			<div class="row space-between">
				<Badge dimension="large" variant="light-overlay">
					<WarningAltFilled size={24} />
					Error
				</Badge>
				<Badge dimension="large" variant="light-overlay">Category</Badge>
			</div>
		</div>
	</section>
</ResponsiveContainer>

<ResponsiveContainer class="vspace">
	<section id="quote" class="controls">
		<div class="quote">
			<Typography font="serif" italic variant="large"
				>“One could describe design as a plan for arranging elements to accomplish a particular
				purpose.”</Typography
			>
			<Typography>Charles Eames</Typography>
		</div>
	</section>

	<Code
		class="landing-code-min-height"
		language="svelte"
		code={`<script lang="ts">
	import Button from '$lib/components/ui/button.svelte'
	import { Close } from 'carbon-icons-svelte'
</` +
			`script>

<Button variant="strong" dimension="default">
	<Close/>Close
</Button>
`}
	/>
</ResponsiveContainer>

<section id="bottom"></section>

<style>
	#introduction {
		margin-top: var(--double-padding);
	}
	#call-to-action {
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
		align-items: center;
		gap: var(--padding);
		margin-top: var(--double-padding);
	}
	#preview-title {
		display: flex;
		align-items: center;
	}
	#size-select {
		display: flex;
		flex: 1;
		justify-content: stretch;
	}

	.controls {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: stretch;
		gap: var(--padding);
		border: var(--colors-low) 1px solid;
		border-radius: var(--border-radius);
		background-color: var(--colors-base);
		padding: var(--double-padding);
	}

	.no-gap {
		gap: 0px;
	}
	.buttons {
		align-items: center;
	}
	.solid {
		background-color: var(--colors-ultra-low);
	}
	:global(.vspace) {
		margin-top: var(--padding);
	}
	:global(.half-margin) {
		margin-top: var(--padding) !important;
		margin-bottom: var(--padding) !important;
	}
	#dark-overlay {
		background-image: url('/backgrounds/noita-digital-QZX8jbK1xEw-unsplash.jpg');
		background-size: cover;
	}
	#light-overlay {
		background-image: url('/backgrounds/augustine-wong-li0iC0rjvvg-unsplash.jpg');
		background-size: cover;
	}
	#bottom {
		margin-top: calc(2 * var(--double-padding));
	}

	.opacity {
		opacity: 0.5;
	}
	.grow {
		flex: 1;
	}
	.row {
		display: flex;
		align-items: center;
		align-self: stretch;
		gap: 0.5rem;
	}
	.space-between {
		justify-content: space-between;
	}
	.col {
		display: flex;
		flex: 1;
		flex-direction: column;
		justify-content: space-between;
		align-self: stretch;
	}
	.col:has(.row.space-between) {
		gap: var(--double-padding);
	}
	.quote {
		display: flex;
		flex-direction: column;
		gap: var(--half-padding);
		margin-top: var(--padding);
		margin-bottom: var(--padding);
		border-left: var(--colors-low) 1px solid;
		padding-left: var(--padding);
	}
	:global(.landing-code-min-height) {
		min-height: initial !important;
	}
</style>
