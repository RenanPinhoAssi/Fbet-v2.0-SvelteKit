<script>
	export let choice_pilot = '';
	export let pilots_choice_name_by_id = {};
	export let choice_score;
	export let result_type = false;
</script>

<div class="dropdown race_position_dropdown">
	<button
		class="
            btn btn-red-dark btn-not-overflow race-bet-button 
            {
                !choice_pilot ? 'not-picked' :
                choice_score == '0'? 'no-match' :
                choice_score == '1'? 'team-match' :
                choice_score == '2'? 'area-match' :
                choice_score == '5'? 'full-match' : ''
            }
        "

		type="button"
	>
		<b>
            {#if choice_pilot}
                {pilots_choice_name_by_id[choice_pilot]}
                {#if !result_type}
                    {choice_score != '1' ? "(+ " + choice_score + " Pontos)" : "(+ 1 Ponto)"}
                {/if}
            {:else}
                Não Selecionado
			{/if}
		</b>
	</button>
</div>
