<script>
	import { api_base_url, stored_token } from '$lib/global.js';
	import { page } from '$app/stores';
	import { onMount } from 'svelte';

	import SectionIdentifier from '../$lib/global/section_identifier.svelte';
	import GrandPrix from '../$lib/schedule/grandprix_card.svelte';

	let tourney_id;
	let grandsprix = [];

	let populate_grands_prix = function () {
		fetch(
			api_base_url +
				'/grandprix/get_grandsprix?' +
				new URLSearchParams({
					tourney_id: tourney_id
				}),
			{
				method: 'GET',
				mode: 'cors',
				headers: {
					Authorization: 'Bearer ' + stored_token,
					Accept: 'application/json'
				}
			}
		)
			.then((response) => response.json())
			.then((data) => {
				grandsprix = data;
			})
			.catch((error) => {
				console.log(error);
				return [];
			});
	};

	onMount(async () => {
		tourney_id = $page.params.tourney;
		populate_grands_prix();
	});
</script>

<SectionIdentifier
	section_name="Cronograma"
	return_button_route_name="Torneios"
	return_button_route_src="/tournaments"
/>

<section class="container-fluid">
	<div class="row">
		<div class="col-sm-10 mx-auto">
			<div class="row" id="tracks-container">
				{#each grandsprix as grandprix}
					<GrandPrix grandprix_info={grandprix} {tourney_id} />
				{/each}
			</div>
		</div>
	</div>
</section>
