<script context="module">
	export function preload() {
		return this.fetch('http://localhost:3001/api/v1/contacts')
			.then(r => r.json())
			.then(contacts => ({ contacts }));
	}
</script>

<script>
	import { goto } from '@sapper/app';

	import { asc, desc } from '../../utils/sort';

	export let contacts = [];
	export let filterByName = '';
	let filteredContacts = contacts;

	$: filteredContacts = contacts.filter(contact => `${contact.firstName} ${contact.lastName}`.toLowerCase().indexOf(filterByName.toLowerCase()) > -1);

	let sortOrderAsc = true;

	$: sortBy = (attrName) => {
		sortOrderAsc = !sortOrderAsc;
		console.info(attrName, sortOrderAsc);

		if (sortOrderAsc) {
			filteredContacts = filteredContacts.sort(asc);
		} else {
			filteredContacts = filteredContacts.sort(desc);
		}
	}
</script>

<svelte:head>
	<title>Blog</title>
</svelte:head>

<style>
	table {
		border-collapse: collapse;
		width: 100%;
	}

	th {
		text-align: left;
	}

	th,
	td {
		border: 1px solid #ccc;
		padding: 5px;
	}

	tbody > tr:hover {
		background-color: #ccc;
		cursor: pointer;
	}

	.filters {
		margin-top: 5px;
		margin-bottom: 10px;
	}

	.field {
		display: flex;
		flex-wrap: wrap;
		flex-direction: row-reverse;
	}

	.field label {
		width: 20%;
	}

	.field input {
		width: 80%;
	}

	.sortable {
		cursor: pointer;
		position: relative;
	}

	.sortable::after {
		content: '';

		width: 0;
		height: 0;

		border-style: solid;
		border-width: 0 7px 7px 7px;
		border-color: transparent transparent #333 transparent;

		position: absolute;

		margin-left: 10px;

		top: 50%;
		transform: translateY(-50%);
	}

	.sortable.asc::after {
		transform: rotateZ(0deg);
	}

	.sortable.desc::after {
		transform: rotateZ(180deg);
	}
</style>

<h1>Contacts</h1>

<div class="filters-container">
	<h2>Filters</h2>
	<div class="filters">
		<div class="field">
			<input type="text" name="filter-by-name" id="filterByName" bind:value={filterByName}>
			<label for="filterByName">Name:</label>
		</div>
	</div>
</div>

<table>
	<thead>
		<tr>
			<th>ID</th>
			<th
				class="sortable {sortOrderAsc === true ? 'asc' : 'desc'}"
				on:click={() => sortBy('name')}
			>Name</th>
			<th>Email</th>
			<th>Actions</th>
		</tr>
	</thead>
	<tbody>
		{#each filteredContacts as contact}
		<tr on:click={() => goto(`/contacts/${contact.id}`)}>
			<td>{contact.id}</td>
			<td>{contact.firstName} {contact.lastName}</td>
			<td>{contact.email}</td>
			<td>Actions</td>
		</tr>
		{/each}
	</tbody>
</table>
