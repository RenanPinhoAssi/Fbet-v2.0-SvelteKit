<script>
	export let choice_pilot = '';
	export let pilots = [];
	export let pilots_choice_name_by_id = {};
	export let disabled = false;

	export let set_bet_choice = function () {};
</script>

<div class="row">
	<div class="col-10 mx-auto">
		<div class="dropdown">
			<button
				class="btn btn-red-dark btn-not-overflow dropdown-toggle classification-bet-button"
				class:disabled
				type="button"
				data-bs-toggle="dropdown"
				aria-haspopup="true"
				aria-expanded="false"
				id="pole_position_button"
			>
				{choice_pilot ? pilots_choice_name_by_id[choice_pilot] : 'Não Selecionado'}
			</button>
			<div class="dropdown-menu" aria-labelledby="poleDropdown" id="pole_position">
				{#each pilots as pilot}
					<a
						class="dropdown-item"
						id="pilot-{pilot.pilot_id}"
						on:click={() => {
							choice_pilot = pilot.pilot_id;
							set_bet_choice('pole_position', pilot.pilot_id);
						}}
					>
						{pilots_choice_name_by_id[pilot.pilot_id]}</a
					>
				{/each}
			</div>
		</div>
	</div>
</div>
