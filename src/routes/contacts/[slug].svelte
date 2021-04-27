<script context="module">
	export async function preload({ params }) {
		// the `slug` parameter is available because
		// this file is called [slug].svelte
		const res = await this.fetch(`http://localhost:3001/api/v1/contacts/${params.slug}`);
		const data = await res.json();

		if (res.status === 200) {
			return { contact: data[0] };
		} else {
			this.error(res.status, data.message);
		}
	}
</script>

<script>
	import Datum from '../../components/Datum.svelte';
	import Date from '../../components/Date.svelte';

	export let contact;
</script>

<style>
	.addresses {
		margin: 5px;
	}

	.address + .address {
		margin-top: 15px;
	}

	.comments {
		color: #444;
		font-style: italic;
	}
</style>

<svelte:head>
	<title>{contact.firstName} {contact.lastName}</title>
</svelte:head>

<h1>{contact.firstName} {contact.lastName}</h1>

<div class="content">
	<Datum label="ID" value={contact.id} />
	<Datum label="Name" value="{contact.firstName} {contact.lastName}" />
	<Datum label="Email" value="{contact.email}" />
	<Date date="{contact.dateOfBirth}" />

	<div class="addresses">
		{#each contact.addresses as address}
		<div class="address">
			<Datum label="Address 1" value={address.address1} />
			<Datum label="Address 2" value={address.address2} />
			<Datum label="Address 3" value={address.address3} />
			<Datum label="City" value={address.city} />
			<Datum label="County" value={address.county} />
			<Datum label="Postcode" value={address.postCode} />
		</div>
		{/each}
	</div>

	<ul class="comments">
		{#each contact.comments as comment}
		<li>{comment}</li>
		{/each}
	</ul>
</div>
