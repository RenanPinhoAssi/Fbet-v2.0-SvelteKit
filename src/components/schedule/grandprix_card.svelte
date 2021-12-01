<script>
	import { goto } from '$app/navigation';

	export let grandprix_info;
	export let tourney_id;
</script>

<div class="col-xs-7 col-sm-6 col-md-4 mx-auto">
	<div class="fbet-card-box" id="fbet-card">
		<div class="track-info">
			<t3 id="race-country">{grandprix_info.country}</t3>
			<p class="race-title">
				<b id="race-title-text">{grandprix_info.gp_name}</b>
			</p>
		</div>

		<div class="track-design">
			<!-- svelte-ignore a11y-missing-attribute -->
			<img class="track-image" id="track-image" src={grandprix_info.map} />
			<p class="fluid-text" id="track-name">{grandprix_info.name}</p>
		</div>

		<div class="classification-datetime">
			<div class="clock-icon" />

			<p>
				<b id="classification-datetime">{grandprix_info.qualifying_date}</b>
			</p>
		</div>

		<div class="race-datetime">
			<div class="flag-icon" />

			<p>
				<b id="race-datetime">{grandprix_info.race_date}</b>
			</p>
		</div>

		<div class="button-box-bottom center-align" id="button-holder">
			{#if grandprix_info.complete == '0'}
				<button
					type="button"
					class="btn btn-red-dark btn-sm w-60 mt-2 mb-2"
					id="bet-button"
					on:click={() => {
						goto('/bet/' + tourney_id + '-' + grandprix_info.id);
					}}
				>
					Apostar
				</button>
			{:else}
				<button
					type="button"
					class="btn btn-black btn-sm w-60 mt-2 mb-2"
					id="bet-button"
					on:click={() => {
						goto('/results/' + tourney_id + '-' + grandprix_info.id);
					}}
				>
					Resultado
				</button>
			{/if}
		</div>
	</div>
</div>
