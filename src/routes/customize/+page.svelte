<script lang="ts">
	import { browser } from '$app/environment'
	import { theme } from '$lib/stores/theme.svelte'
	import Typography from '$lib/components/ui/typography.svelte'
	import Button from '$lib/components/ui/button.svelte'
	import Input from '$lib/components/ui/input/input.svelte'
	import ColorInput from '$lib/components/ui/input/color-input.svelte'
	import Select from '$lib/components/ui/select/select.svelte'
	import Option from '$lib/components/ui/select/option.svelte'
	import RadioGroup from '$lib/components/ui/radio-button/radio-group.svelte'
	import Radio from '$lib/components/ui/radio-button/radio.svelte'
	import Slider from '$lib/components/ui/slider.svelte'
	import { Reset, Download, Copy } from 'carbon-icons-svelte'
	import { onMount, onDestroy } from 'svelte'

	// Design token states
	let baseColor = $state('#fefefe')
	let mode = $state('system')
	let fontFamily = $state('sans-serif')
	let fontSize = $state(16)
	let padding = $state(16)
	let borderRadius = $state(4)
	let focusOutlineWidth = $state(4)

	// Observer for style changes
	let observer: MutationObserver | undefined
	let isUpdatingFromStore = false

	// Predefined themes
	const predefinedThemes = [
		{
			name: 'Default',
			description: 'Clean and minimal design',
			baseColor: '#fefefe',
			mode: 'system',
			fontFamily: 'sans-serif',
			fontSize: 16,
			padding: 16,
			borderRadius: 4,
			focusOutlineWidth: 4,
		},
		{
			name: 'Blue Ocean',
			description: 'Professional blue theme',
			baseColor: '#1890ff',
			mode: 'light',
			fontFamily: 'sans-serif',
			fontSize: 16,
			padding: 16,
			borderRadius: 6,
			focusOutlineWidth: 4,
		},
		{
			name: 'Forest Green',
			description: 'Natural and calming',
			baseColor: '#52c41a',
			mode: 'light',
			fontFamily: 'sans-serif',
			fontSize: 16,
			padding: 18,
			borderRadius: 8,
			focusOutlineWidth: 3,
		},
		{
			name: 'Sunset Orange',
			description: 'Warm and energetic',
			baseColor: '#fa8c16',
			mode: 'light',
			fontFamily: 'sans-serif',
			fontSize: 16,
			padding: 16,
			borderRadius: 6,
			focusOutlineWidth: 4,
		},
		{
			name: 'Royal Purple',
			description: 'Premium and elegant',
			baseColor: '#722ed1',
			mode: 'light',
			fontFamily: 'serif',
			fontSize: 17,
			padding: 20,
			borderRadius: 8,
			focusOutlineWidth: 4,
		},
		{
			name: 'Cherry Red',
			description: 'Bold and attention-grabbing',
			baseColor: '#f5222d',
			mode: 'light',
			fontFamily: 'sans-serif',
			fontSize: 16,
			padding: 16,
			borderRadius: 4,
			focusOutlineWidth: 5,
		},
		{
			name: 'Dark Mode',
			description: 'Modern dark interface',
			baseColor: '#177ddc',
			mode: 'dark',
			fontFamily: 'sans-serif',
			fontSize: 16,
			padding: 16,
			borderRadius: 6,
			focusOutlineWidth: 4,
		},
		{
			name: 'Minimalist',
			description: 'Ultra-clean with subtle borders',
			baseColor: '#f0f0f0',
			mode: 'light',
			fontFamily: 'sans-serif',
			fontSize: 15,
			padding: 12,
			borderRadius: 2,
			focusOutlineWidth: 2,
		},
		{
			name: 'Rounded & Soft',
			description: 'Friendly rounded interface',
			baseColor: '#13c2c2',
			mode: 'light',
			fontFamily: 'sans-serif',
			fontSize: 16,
			padding: 20,
			borderRadius: 12,
			focusOutlineWidth: 4,
		},
		{
			name: 'Tech Monospace',
			description: 'Developer-focused theme',
			baseColor: '#2f54eb',
			mode: 'light',
			fontFamily: 'monospace',
			fontSize: 15,
			padding: 14,
			borderRadius: 4,
			focusOutlineWidth: 3,
		},
		{
			name: 'Vintage Sepia',
			description: 'Classic and timeless',
			baseColor: '#d4b106',
			mode: 'light',
			fontFamily: 'serif',
			fontSize: 17,
			padding: 18,
			borderRadius: 6,
			focusOutlineWidth: 4,
		},
		{
			name: 'High Contrast',
			description: 'Maximum accessibility',
			baseColor: '#000000',
			mode: 'light',
			fontFamily: 'sans-serif',
			fontSize: 18,
			padding: 20,
			borderRadius: 0,
			focusOutlineWidth: 6,
		},
	] as const

	// CSS generation
	let customCSS = $derived.by(() => {
		return `/* Custom Diète Design System */
:root {
	/* Typography */
	--font-family-sans-serif: ${fontFamily === 'custom' ? 'Inter, system-ui, -apple-system, sans-serif' : fontFamily};

	/* Font Sizes */
	--font-size: ${fontSize / 16}rem;
	--font-size-large: ${(fontSize * 1.5) / 16}rem;
	--font-size-small: ${(fontSize * 0.75) / 16}rem;

	/* Line Heights */
	--line-height: ${(fontSize * 1.5) / 16}rem;
	--line-height-large: ${(fontSize * 2) / 16}rem;
	--line-height-small: ${fontSize / 16}rem;

	/* Spacing */
	--padding: ${padding / 16}rem;
	--quarter-padding: ${padding / 4 / 16}rem;
	--half-padding: ${padding / 2 / 16}rem;
	--three-quarters-padding: ${(padding * 0.75) / 16}rem;
	--one-and-a-half-padding: ${(padding * 1.5) / 16}rem;
	--double-padding: ${(padding * 2) / 16}rem;

	/* Border Radius */
	--border-radius: ${borderRadius / 16}rem;

	/* Focus Outline */
	--focus-outline: ${focusOutlineWidth}px solid var(--colors-top);
	--focus-outline-offset: -${focusOutlineWidth}px;
}

/* Base Color: ${baseColor} */
/* Mode: ${mode} */
/* Colors are automatically calculated based on your base color and mode selection */`
	})

	// Apply changes to CSS custom properties
	function applyCustomizations() {
		if (!browser) return

		const root = document.documentElement

		// Apply typography
		root.style.setProperty(
			'--font-family-sans-serif',
			fontFamily === 'custom' ? 'Inter, system-ui, -apple-system, sans-serif' : fontFamily,
		)

		// Apply font sizes
		root.style.setProperty('--font-size', `${fontSize / 16}rem`)
		root.style.setProperty('--font-size-large', `${(fontSize * 1.5) / 16}rem`)
		root.style.setProperty('--font-size-small', `${(fontSize * 0.75) / 16}rem`)

		// Apply line heights
		root.style.setProperty('--line-height', `${(fontSize * 1.5) / 16}rem`)
		root.style.setProperty('--line-height-large', `${(fontSize * 2) / 16}rem`)
		root.style.setProperty('--line-height-small', `${fontSize / 16}rem`)

		// Apply spacing
		root.style.setProperty('--padding', `${padding / 16}rem`)
		root.style.setProperty('--quarter-padding', `${padding / 4 / 16}rem`)
		root.style.setProperty('--half-padding', `${padding / 2 / 16}rem`)
		root.style.setProperty('--three-quarters-padding', `${(padding * 0.75) / 16}rem`)
		root.style.setProperty('--one-and-a-half-padding', `${(padding * 1.5) / 16}rem`)
		root.style.setProperty('--double-padding', `${(padding * 2) / 16}rem`)

		// Apply border radius
		root.style.setProperty('--border-radius', `${borderRadius / 16}rem`)

		// Apply focus outline
		root.style.setProperty('--focus-outline', `${focusOutlineWidth}px solid var(--colors-top)`)
		root.style.setProperty('--focus-outline-offset', `-${focusOutlineWidth}px`)

		// Save to localStorage
		localStorage.setItem(
			'customizations',
			JSON.stringify({
				fontFamily,
				fontSize,
				padding,
				borderRadius,
				focusOutlineWidth,
			}),
		)
	}

	// Load customizations from localStorage
	function loadCustomizations() {
		if (!browser) return

		// Initialize from theme store
		isUpdatingFromStore = true
		baseColor = theme.baseColor
		mode = theme.mode
		isUpdatingFromStore = false

		// Load saved customizations
		const saved = localStorage.getItem('customizations')
		if (saved) {
			try {
				const parsed = JSON.parse(saved)
				fontFamily = parsed.fontFamily || 'sans-serif'
				fontSize = parsed.fontSize || 16
				padding = parsed.padding || 16
				borderRadius = parsed.borderRadius || 4
				focusOutlineWidth = parsed.focusOutlineWidth || 4
			} catch (e) {
				console.warn('Failed to load customizations:', e)
			}
		}

		applyCustomizations()
	}

	// Reset to defaults
	function resetToDefaults() {
		theme.baseColor = '#fefefe'
		theme.mode = 'system'
		baseColor = '#fefefe'
		mode = 'system'
		fontFamily = 'sans-serif'
		fontSize = 16
		padding = 16
		borderRadius = 4
		focusOutlineWidth = 4

		if (browser) {
			localStorage.removeItem('customizations')
			const root = document.documentElement

			// Reset CSS properties to default values
			root.style.removeProperty('--font-family-sans-serif')
			root.style.removeProperty('--font-size')
			root.style.removeProperty('--font-size-large')
			root.style.removeProperty('--font-size-small')
			root.style.removeProperty('--line-height')
			root.style.removeProperty('--line-height-large')
			root.style.removeProperty('--line-height-small')
			root.style.removeProperty('--padding')
			root.style.removeProperty('--quarter-padding')
			root.style.removeProperty('--half-padding')
			root.style.removeProperty('--three-quarters-padding')
			root.style.removeProperty('--one-and-a-half-padding')
			root.style.removeProperty('--double-padding')
			root.style.removeProperty('--border-radius')
			root.style.removeProperty('--focus-outline')
			root.style.removeProperty('--focus-outline-offset')
		}
	}

	// Copy CSS to clipboard
	async function copyCSS() {
		if (!browser) return

		try {
			await navigator.clipboard.writeText(customCSS)
			alert('CSS copied to clipboard!')
		} catch (err) {
			console.error('Failed to copy CSS:', err)
		}
	}

	// Download CSS file
	function downloadCSS() {
		if (!browser) return

		const blob = new Blob([customCSS], { type: 'text/css' })
		const url = URL.createObjectURL(blob)
		const a = document.createElement('a')
		a.href = url
		a.download = 'diete-custom.css'
		a.click()
		URL.revokeObjectURL(url)
	}

	// Apply a predefined theme
	function applyPredefinedTheme(themeConfig: typeof predefinedThemes[0]) {
		baseColor = themeConfig.baseColor
		mode = themeConfig.mode as 'light' | 'dark' | 'system'
		fontFamily = themeConfig.fontFamily
		fontSize = themeConfig.fontSize
		padding = themeConfig.padding
		borderRadius = themeConfig.borderRadius
		focusOutlineWidth = themeConfig.focusOutlineWidth
		
		updateThemeStore()
	}

	// Update theme store when local values change
	function updateThemeStore() {
		if (isUpdatingFromStore) return

		if (theme.baseColor !== baseColor) {
			theme.baseColor = baseColor
		}
		if (theme.mode !== mode) {
			theme.mode = mode
		}

		applyCustomizations()
	}

	// Lifecycle management
	onMount(() => {
		loadCustomizations()

		// Set up MutationObserver to watch for external theme changes
		if (browser) {
			observer = new MutationObserver((mutations) => {
				mutations.forEach((mutation) => {
					if (
						mutation.type === 'attributes' &&
						mutation.attributeName === 'style' &&
						mutation.target === document.documentElement
					) {
						// Theme changed externally, sync our local state
						if (!isUpdatingFromStore && theme.baseColor !== baseColor) {
							isUpdatingFromStore = true
							baseColor = theme.baseColor
							isUpdatingFromStore = false
						}
						if (!isUpdatingFromStore && theme.mode !== mode) {
							isUpdatingFromStore = true
							mode = theme.mode
							isUpdatingFromStore = false
						}
					}
				})
			})
			observer.observe(document.documentElement, {
				attributes: true,
				attributeFilter: ['style'],
			})
		}
	})

	onDestroy(() => {
		if (observer) {
			observer.disconnect()
		}
	})
</script>

<div class="customize-page">
	<div class="header">
		<Typography variant="h1">Customize Diète</Typography>
		<Typography>
			Customize the design system to match your brand and preferences. Changes are applied in
			real-time and saved automatically. You can export your customizations as CSS or reset to
			defaults.
		</Typography>
	</div>

	<section class="section full-width">
		<Typography variant="h3">Predefined Themes</Typography>
		<Typography variant="small" class="theme-description">
			Quick start with professionally designed themes. Click any theme to apply it instantly.
		</Typography>
		<div class="theme-grid">
			{#each predefinedThemes as theme}
				<div 
					class="theme-card" 
					onclick={() => applyPredefinedTheme(theme)}
					role="button" 
					tabindex="0"
					onkeydown={(e) => e.key === 'Enter' && applyPredefinedTheme(theme)}
				>
					<div class="theme-preview" style="background-color: {theme.baseColor}"></div>
					<div class="theme-info">
						<Typography variant="small" bold>{theme.name}</Typography>
						<Typography variant="small" class="theme-desc">{theme.description}</Typography>
						<div class="theme-details">
							<span class="theme-detail">{theme.mode}</span>
							<span class="theme-detail">{theme.fontFamily}</span>
							<span class="theme-detail">{theme.fontSize}px</span>
						</div>
					</div>
				</div>
			{/each}
		</div>
	</section>

	<div class="content">
		<div class="controls">
			<section class="section">
				<Typography variant="h3">Colors & Theme</Typography>
				<div class="form-group">
					<ColorInput bind:value={baseColor} label="Base Color" onchange={updateThemeStore} />
				</div>
				<div class="form-group">
					<Typography variant="small" bold>Theme Mode</Typography>
					<RadioGroup bind:value={mode} onchange={updateThemeStore}>
						<Radio value="light">Light</Radio>
						<Radio value="dark">Dark</Radio>
						<Radio value="system">System</Radio>
					</RadioGroup>
				</div>
			</section>

			<section class="section">
				<Typography variant="h3">Typography</Typography>
				<div class="form-group">
					<Select bind:value={fontFamily} label="Font Family" onchange={updateThemeStore}>
						<Option value="sans-serif">System Sans Serif</Option>
						<Option value="serif">System Serif</Option>
						<Option value="monospace">System Monospace</Option>
						<Option value="custom">Custom (Inter)</Option>
					</Select>
				</div>
				<div class="form-group">
					<Typography variant="small" bold>Base Font Size: {fontSize}px</Typography>
					<Slider
						bind:value={fontSize}
						min={12}
						max={24}
						step={1}
						label="Base Font Size"
						onchange={updateThemeStore}
					/>
				</div>
			</section>

			<section class="section">
				<Typography variant="h3">Spacing & Layout</Typography>
				<div class="form-group">
					<Typography variant="small" bold>Base Padding: {padding}px</Typography>
					<Slider
						bind:value={padding}
						min={8}
						max={32}
						step={2}
						label="Base Padding"
						onchange={updateThemeStore}
					/>
				</div>
				<div class="form-group">
					<Typography variant="small" bold>Border Radius: {borderRadius}px</Typography>
					<Slider
						bind:value={borderRadius}
						min={0}
						max={16}
						step={1}
						label="Border Radius"
						onchange={updateThemeStore}
					/>
				</div>
				<div class="form-group">
					<Typography variant="small" bold>Focus Outline Width: {focusOutlineWidth}px</Typography>
					<Slider
						bind:value={focusOutlineWidth}
						min={1}
						max={8}
						step={1}
						label="Focus Outline Width"
						onchange={updateThemeStore}
					/>
				</div>
			</section>

			<section class="section">
				<Typography variant="h3">Actions</Typography>
				<div class="actions">
					<Button variant="secondary" onclick={resetToDefaults}>
						<Reset size={16} />
						Reset to Defaults
					</Button>
					<Button variant="ghost" onclick={copyCSS}>
						<Copy size={16} />
						Copy CSS
					</Button>
					<Button variant="strong" onclick={downloadCSS}>
						<Download size={16} />
						Download CSS
					</Button>
				</div>
			</section>
		</div>

		<div class="preview">
			<section class="section">
				<Typography variant="h3">Live Preview</Typography>
				<Typography variant="small">
					See how your customizations look with real components:
				</Typography>

				<div class="preview-content">
					<div class="component-preview">
						<Typography variant="h4">Typography</Typography>
						<Typography>This is regular text with your custom font settings.</Typography>
						<Typography variant="small">Small text variant</Typography>
					</div>

					<div class="component-preview">
						<Typography variant="h4">Buttons</Typography>
						<div class="button-group">
							<Button variant="strong">Strong Button</Button>
							<Button variant="secondary">Secondary Button</Button>
							<Button variant="ghost">Ghost Button</Button>
						</div>
					</div>

					<div class="component-preview">
						<Typography variant="h4">Form Controls</Typography>
						<Input label="Text Input" placeholder="Type something..." />
						<div class="spacing-demo">
							<div class="spacing-box">Padding Demo</div>
						</div>
					</div>
				</div>
			</section>

			<section class="section">
				<Typography variant="h3">Generated CSS</Typography>
				<div class="css-output">
					<pre><code>{customCSS}</code></pre>
				</div>
			</section>
		</div>
	</div>
</div>

<style lang="postcss">
	.customize-page {
		margin: 0 auto;
		padding: var(--double-padding);
		max-width: 1200px;
	}

	.header {
		margin-bottom: var(--double-padding);
	}

	.content {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: var(--double-padding);
	}

	.section {
		margin-bottom: var(--double-padding);
	}

	.full-width {
		width: 100%;
		margin-bottom: var(--double-padding);
	}

	.form-group {
		margin-bottom: var(--padding);
	}

	.actions {
		display: flex;
		flex-wrap: wrap;
		gap: var(--half-padding);
	}

	.preview-content {
		border: 1px solid var(--colors-low);
		border-radius: var(--border-radius);
		background: var(--colors-ultra-low);
		padding: var(--padding);
	}

	.component-preview {
		margin-bottom: var(--padding);

		&:last-child {
			margin-bottom: 0;
		}
	}

	.button-group {
		display: flex;
		flex-wrap: wrap;
		gap: var(--half-padding);
	}

	.spacing-demo {
		margin-top: var(--half-padding);
	}

	.spacing-box {
		border-radius: var(--border-radius);
		background: var(--colors-high);
		padding: var(--padding);
		color: var(--colors-base);
		font-size: var(--font-size-small);
		text-align: center;
	}

	.css-output {
		border: 1px solid var(--colors-low);
		border-radius: var(--border-radius);
		background: var(--colors-ultra-low);
		padding: var(--padding);
		max-height: 300px;
		overflow-y: auto;

		pre {
			margin: 0;
			font-size: var(--font-size-small);
			line-height: var(--line-height-small);
			font-family: var(--font-family-monospace);
			white-space: pre-wrap;
			word-wrap: break-word;
		}

		code {
			font-family: inherit;
		}
	}

	.theme-description {
		margin-bottom: var(--padding);
		opacity: 0.8;
	}

	.theme-grid {
		display: flex;
		flex-wrap: wrap;
		gap: var(--half-padding);
		margin-bottom: var(--padding);
	}

	.theme-card {
		flex: 1 1 200px;
		min-width: 180px;
		max-width: 250px;
		border: 1px solid var(--colors-low);
		border-radius: var(--border-radius);
		padding: var(--half-padding);
		cursor: pointer;
		transition: all 0.2s ease;
		background: var(--colors-base);

		&:hover {
			border-color: var(--colors-high);
			box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
			transform: translateY(-2px);
		}

		&:focus-visible {
			outline: var(--focus-outline);
			outline-offset: var(--focus-outline-offset);
		}
	}

	.theme-preview {
		width: 100%;
		height: 40px;
		border-radius: calc(var(--border-radius) - 2px);
		margin-bottom: var(--half-padding);
		border: 1px solid var(--colors-low);
	}

	.theme-info {
		display: flex;
		flex-direction: column;
		gap: var(--quarter-padding);
	}

	.theme-desc {
		opacity: 0.7;
		font-size: var(--font-size-small);
		line-height: var(--line-height-small);
	}

	.theme-details {
		display: flex;
		gap: var(--quarter-padding);
		flex-wrap: wrap;
	}

	.theme-detail {
		background: var(--colors-ultra-low);
		border: 1px solid var(--colors-low);
		border-radius: calc(var(--border-radius) / 2);
		padding: 2px 6px;
		font-size: var(--font-size-small);
		color: var(--colors-high);
	}

	@media (max-width: 768px) {
		.content {
			grid-template-columns: 1fr;
		}

		.customize-page {
			padding: var(--padding);
		}

		.theme-card {
			flex: 1 1 160px;
			min-width: 140px;
			max-width: none;
		}
	}
</style>
