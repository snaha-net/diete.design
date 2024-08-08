<script>
	import Typography from '$lib/components/ui/typography.svelte'
	import { Information } from 'carbon-icons-svelte'
</script>

<div class="flex">
	<Typography variant="small">small text</Typography>
	<Typography variant="small"><Information size={16} />small text with icon</Typography>
</div>

<style lang="postcss">
	.flex {
		display: flex;
	}
</style>
