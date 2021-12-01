<script>
	import { onMount } from 'svelte';
	import { goto } from '$app/navigation';
	import { page } from '$app/stores';

	import SectionIdentifier from '../../components/global/section_identifier.svelte';
	import ShortIdentifier from '../../components/global/short_identifier.svelte';
	import PolePositionSlot from '../../components/bet/pole_position_slot.svelte';
	import RacePositionSlot from '../../components/bet/race_position_slot.svelte';

	let pilots = [];
	let pilots_choice_name_by_id = {};
	let choices = {};
	let saving = false;

	let grandprix_id = $page.params.grandprix;
	let tourney_id = $page.params.tourney;
	let grandprix_info = {};

	let classification_date_expired = false;
	let race_date_expired = false;

	let set_bet_choice = function (position, pilot_id) {
		saving = true;
		let bet_choice = {
			grandprix: grandprix_id,
			tourney_id: tourney_id
		};
		bet_choice[position] = pilot_id;

		fetch(api_base_url + '/bets/register_bet', {
			method: 'POST',
			mode: 'cors',
			headers: {
				Authorization: 'Bearer ' + stored_token,
				Accept: 'application/json'
			},
			body: JSON.stringify(bet_choice)
		})
			.then((response) => response.json())
			.then((data) => {
				console.log(data);
				setTimeout(function () {
					saving = false;
				}, 500);
			})
			.catch((error) => {
				console.log(error);
				return [];
			});
	};

	let get_choices_bet = function () {
		fetch(
			api_base_url +
				'/bets/get_bet_choices?' +
				new URLSearchParams({
					grandprix_id: grandprix_id,
					tourney_id: 1
				}),
			{
				method: 'GET',
				mode: 'cors',
				headers: {
					Authorization: 'Bearer ' + stored_token
				}
			}
		)
			.then((response) => response.json())
			.then((data) => {
				choices = data;
				console.log(data);
			})
			.catch((error) => {
				console.log(error);
				return [];
			});
	};

	let setup_pilot_options = function () {
		fetch(api_base_url + '/teams/get_full_teams', {
			method: 'GET',
			mode: 'cors'
		})
			.then((response) => response.json())
			.then((data) => {
				pilots = data;
				for (var i in pilots) {
					pilots_choice_name_by_id[pilots[i].pilot_id] =
						pilots[i].pilot_surname + ' (' + pilots[i].pilot_number + ') - ' + pilots[i].team_name;
				}
			})
			.catch((error) => {
				console.log(error);
				return [];
			});
	};

	let get_grandprix_info = function () {
		fetch(
			api_base_url +
				'/grandprix/get_grandprix?' +
				new URLSearchParams({
					id: grandprix_id
				}),
			{
				method: 'GET',
				mode: 'cors'
			}
		)
			.then((response) => response.json())
			.then((data) => {
				let current_date = new Date();

				grandprix_info = data['general_info'];
				if (new Date(grandprix_info['qualifying_date']) < current_date) {
					classification_date_expired = true;
				}
				if (new Date(grandprix_info['race_date']) < current_date) {
					race_date_expired = true;
				}
			})
			.catch((error) => {
				console.log(error);
				return [];
			});
	};

	onMount(async () => {
		get_grandprix_info();
		get_choices_bet();
		setup_pilot_options();
	});
</script>

<SectionIdentifier
	section_name="Apostas"
	section_description={grandprix_info['country'] +
		' - ' +
		grandprix_info['season'] +
		' - ' +
		grandprix_info['name']}
	return_button_route_name="Cronograma"
	return_button_route_src={"/schedule/"+tourney_id}
/>

<section class="container-fluid mb-5" id="bet-section">
	<div class="row">
		<div class="col-sm-10 mx-auto">
			<ShortIdentifier name="Pole-Position" />
		</div>
	</div>

	<PolePositionSlot
		choice_pilot={choices['pole_position']}
		{pilots}
		{pilots_choice_name_by_id}
		{set_bet_choice}
		disabled={classification_date_expired}
	/>
	<div class="row">
		<div class="col-sm-10 mx-auto">
			<ShortIdentifier name="Resultado da Corrida" />
		</div>
	</div>

	<div class="row">
		<div class="col-sm-10 mx-auto" class:disabled={race_date_expired}>
			{#each Array(10) as position, i}
				<RacePositionSlot
					position={i + 1}
					choice_pilot={choices['race_position_' + (i + 1)]}
					{pilots}
					{pilots_choice_name_by_id}
					{set_bet_choice}
					disabled={race_date_expired}
				/>
			{/each}
		</div>
	</div>

	<div class="row mt-5">
		<div class="col-sm-10 mx-auto">
			<button
				type="button"
				class="btn btn-black btn-sm mb-1 w-100"
				on:click={() => {
					goto("/schedule/"+tourney_id);
				}}
			>
				Voltar ao cronograma
			</button>
		</div>
	</div>

	<div class="save-icon" class:active={saving} />
</section>
