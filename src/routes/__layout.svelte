<script>
	import Header from '../components/global/Header.svelte';
	import { page } from '$app/stores';

	let menu_on = true;

	let control_menu = function () {
		if ($page.path == '/login') {
			menu_on = false;
		} else {
			menu_on = true;
		}
	};
</script>

<Header visible={menu_on} />

<main>
	<slot />
</main>

<svelte:window
	on:sveltekit:start={() => {
		console.log('App started!');
		control_menu();
	}}
	on:sveltekit:navigation-start={() => {
		console.log('Navigation started!');
		control_menu();
	}}
	on:sveltekit:navigation-end={() => {
		console.log('Navigation ended!');
		control_menu();
	}}
/>
