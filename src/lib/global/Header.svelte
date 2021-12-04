<script>
	import { goto } from '$app/navigation';
	export let visible;
</script>

<header style="display: {visible ? 'block' : 'none'};">
	<div id="menu-viewer">
		<div class="menu">
			<div class="logo-fbet menu-logo" />

			<div
				class="logout-button"
				on:click={() => {
					goto('/login');
					localStorage.clear();
				}}
			/>
		</div>
	</div>
</header>
