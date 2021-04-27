<script>
	export let date;

	const parsedDate = new Date(date);
	const prettyDate = parsedDate.toISOString().split('T')[0];
</script>

<style></style>

{prettyDate}
