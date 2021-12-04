<script>
	import { goto } from '$app/navigation';

	export let section_name = '';
	export let section_description = '';
	export let return_button_route_name = '';
	export let return_button_route_src = '';
</script>

<section class="container-fluid" id="info-section">
	<div class="row">
		<div class="col-sm-10 mx-auto">
			{#if return_button_route_name}
				<button
					type="button"
					class="btn btn-red-dark btn-sm mb-1"
					style="width: 29%; float: right;"
					on:click={() => {
						goto(return_button_route_src);
					}}
				>
					{return_button_route_name}
				</button>
			{/if}
			<t1>{section_name}</t1>
			{#if section_description}
				<t2 class="my-5">{section_description}</t2>
			{/if}
		</div>
	</div>
</section>
