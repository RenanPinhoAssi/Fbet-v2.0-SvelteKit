<script>
	import { onMount } from 'svelte';
	import { page } from '$app/stores';

	import SectionIdentifier from '../../components/global/section_identifier.svelte';
	import ShortIdentifier from '../../components/global/short_identifier.svelte';
	import ResultSlot from '../../components/results/result_slot.svelte';

	let pilots = [];
	let pilots_choice_name_by_id = {};
	let choices = {};
	let saving = false;

	let tourney_id = $page.params.tourney;
	let grandprix_id = $page.params.grandprix;
	let grandprix_info = {};
	let grandprix_result = {};

	let get_choices_bet = function () {
		fetch(
			api_base_url +
				'/bets/get_bet?' +
				new URLSearchParams({
					grandprix_id: grandprix_id,
					tourney_id: tourney_id
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
						'[' + pilots[i].team_name + '] ' + pilots[i].pilot_name + ' ' + pilots[i].pilot_surname;
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
				grandprix_result = data['result'];
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
	section_name="Resultados"
	section_description={grandprix_info['country'] +
		' - ' +
		grandprix_info['season'] +
		' - ' +
		grandprix_info['name']}
	return_button_route_name="Cronograma"
	return_button_route_src={"/schedule/"+tourney_id}
/>

<section class="container-fluid mb-5" id="info-section">
	<div class="row">
		<div class="col-sm-10 mx-auto">
			<div class="row">
				<div class="col-6">
					<ShortIdentifier name="Resultado da Pole-Position" />
				</div>
				<div class="col-6">
					<ShortIdentifier name="Minha Aposta" />
				</div>
			</div>
			<div class="row">
				<div class="col-6">
					<ResultSlot
						choice_pilot={grandprix_result['pole_position']}
						{pilots_choice_name_by_id}
						result_type={true}
					/>
				</div>
				<div class="col-6">
					<ResultSlot
						choice_pilot={choices['pole_position']}
						{pilots_choice_name_by_id}
						choice_score={choices['points_pole_position']}
					/>
				</div>
			</div>
		</div>
	</div>
</section>

<section class="container-fluid mb-5" id="info-section">
	<div class="row">
		<div class="col-sm-10 mx-auto">
			<div class="row">
				<div class="col-6">
					<ShortIdentifier name="Resultado da Corrida" />
				</div>
				<div class="col-6">
					<ShortIdentifier name="Minha Aposta" />
				</div>
			</div>

			{#each Array(10) as position, i}
				<div class="row">
					<div class="col-12">
						<h4>{i + 1}° Lugar</h4>
					</div>
					<div class="col-6">
						<ResultSlot
							choice_pilot={grandprix_result['race_position_' + (i + 1)]}
							{pilots_choice_name_by_id}
							result_type={true}
						/>
					</div>
					<div class="col-6">
						<ResultSlot
							choice_pilot={choices['race_position_' + (i + 1)]}
							{pilots_choice_name_by_id}
							choice_score={choices['points_position_' + (i + 1)]}
						/>
					</div>
				</div>
			{/each}
		</div>
	</div>
</section>
