<script>
	export let label;
	export let value;
</script>

<style>
	.datum {
		margin: 5px 0;
	}
	.label {
		font-weight: bold;
	}
</style>

{#if value}
<div class="datum">
	{#if label}
	<span class="label">{label}</span>
	{/if}
	{value}
</div>
{/if}
