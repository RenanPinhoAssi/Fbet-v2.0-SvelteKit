<script>
	import { api_base_url, stored_token } from '$lib/global.js';
	import { goto } from '$app/navigation';
	import { onMount } from 'svelte';

	export let tourney_info;

	let tourney_users = [];

	let get_tourney_users = function () {
		fetch(
			api_base_url +
				'/tournament/get_tourney_users?' +
				new URLSearchParams({
					tourney: tourney_info.id
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
				tourney_users = data;
			})
			.catch((error) => {
				console.log(error);
				return [];
			});
	};

	onMount(async () => {
		if (tourney_info.registered == '1') {
			get_tourney_users();
		}
	});
</script>

<div class="col-12 col-sm-10 mx-auto my-4">
	<div class="fbet-card-box" id="torney-box">
		<div class="tournament-header">
			<t3 id="tournament-name">{tourney_info.name}</t3>
		</div>
		<div class="tournament-info-design center-align">
			<!-- <img class="track-image" id="tournament-image" src="" /> -->
		</div>
		<div>
			<p style="color:black" id="tournament-description" />
		</div>
		<div class="tournament-header mt-3" id="standings-holder">
			{#if tourney_info.registered == '1'}
				<t3 id="tournament-table">Tabela do Campeonato</t3>
				<table class="table table-bordered table-user-standings">
					<thead>
						<tr>
							<th class="center-align">#</th>
							<th class="w-80">Usuário</th>
							<th>Pontuação</th>
						</tr>
						{#each tourney_users as user, i}
							<tr>
								<td class="center-align" id="user-position">{i + 1}</td>
								<td id="user-name">{user.nickname}</td>
								<td class="center-align" id="user-score">{user.score}</td>
							</tr>
						{/each}
					</thead>
					<tbody id="table-content" />
				</table>
			{/if}
		</div>
		<div class="button-box-bottom center-align" id="button-holder">
			{#if tourney_info.registered == '1'}
				<button
					type="button"
					class="btn btn-red-dark btn-sm w-60 mt-2 mb-2"
					id="subscribe-button"
					on:click={() => {
						goto('/schedule/' + tourney_info.id);
					}}>Eventos</button
				>
			{:else}
				<button type="button" class="btn btn-red-dark btn-sm w-60 mt-2 mb-2" id="subscribe-button"
					>Inscrever-se</button
				>
			{/if}
		</div>
	</div>
</div>
