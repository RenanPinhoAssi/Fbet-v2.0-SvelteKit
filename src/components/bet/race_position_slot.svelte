<script>
	export let position;
	export let choice_pilot = '';
	export let pilots = [];
	export let pilots_choice_name_by_id = {};
	export let disabled = false;

	export let set_bet_choice = function () {};

	let position_suffix = '° Lugar';
</script>

<div class="row">
	<div class="col-10 col-sm-6 mx-auto {position % 2 == 0 ? 'ms-sm-auto me-sm-0' : 'mx-sm-0'}">
		<h4>{position + position_suffix}</h4>
		<div class="dropdown race_position_dropdown">
			<button
				class="btn btn-red-dark btn-not-overflow dropdown-toggle race-bet-button not-large"
				class:disabled
				type="button"
				data-bs-toggle="dropdown"
				aria-haspopup="true"
				aria-expanded="false"
				id="race_position_{position}_button"
			>
				{choice_pilot ? pilots_choice_name_by_id[choice_pilot] : 'Não Selecionado'}
			</button>
			<div class="dropdown-menu" aria-labelledby="poleDropdown" id="race_position_{position}">
				{#each pilots as pilot}
					<a
						class="dropdown-item"
						id="pilot-{(position, pilot.pilot_id)}"
						on:click={() => {
							choice_pilot = pilot.pilot_id;
							set_bet_choice('race_position_' + position, pilot.pilot_id);
						}}
					>
						{pilots_choice_name_by_id[pilot.pilot_id]}</a
					>
				{/each}
			</div>
		</div>
	</div>
</div>
