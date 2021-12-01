<script>
	import { onMount } from 'svelte';
	import { goto } from '$app/navigation';

	let account = {
		username: 'Tester',
		password: 'Tester123'
	};

	let login = function () {
		console.log(account);
		fetch(api_base_url + '/login/signin', {
			method: 'POST',
			mode: 'cors',
			body: JSON.stringify(account)
		})
			.then((response) => response.json())
			.then((data) => {
				stored_token = data['token'];
				localStorage.setItem('token', data['token']);
				goto(main_route);
			})
			.catch((error) => {
				console.log(error);
				return [];
			});

		// $http({
		// 	method: 'POST',
		// 	url: api_base_url + '/login/signin',
		// 	data: JSON.stringify($scope.account)
		// }).then(function successCallback(response) {
		// 	let data = response.data;
		// 	tokenChecker.setUser(data['token']);
		// 	localStorage.setItem('token', data['token']);
		// 	$location.path(main_route);
		// });
	};
</script>

<section class="container-fluid" id="f1-background">
	<div class="row">
		<div id="login-box">
			<div class="col-sm-10 mx-auto mt-3 mb-2">
				<div class="logo-fbet" />
			</div>

			<div class="col-sm-10 mx-auto">
				<p class="m-0">Nome da Conta</p>
				<input
					class="no-select form-control"
					placeholder="Nome da Conta"
					id="user-form-field"
					bind:value={account['username']}
				/>
				<p class="m-0">Senha</p>
				<input
					type="password"
					class="no-select form-control"
					placeholder="Senha"
					id="password-form-field"
					bind:value={account['password']}
				/>
				<span id="email-field">
					<p class="m-0">E-Mail</p>
					<input class="no-select form-control" placeholder="E-Mail" id="email-form-field" />
					<p class="m-0">Pseudônimo</p>
					<input class="no-select form-control" placeholder="Pseudônimo" id="nickname-form-field" />
				</span>
				<span id="code-field">
					<p class="m-0">Código de Ativação</p>
					<input
						class="no-select form-control"
						placeholder="Código de Ativação"
						id="code-form-field"
					/>
				</span>
			</div>

			<div class="col-sm-10 mx-auto bottom-align" id="login-page-buttons">
				<button type="button" class="btn btn-black btn-sm w-100 mb-1" on:click={login}>
					Entrar
				</button>
				<!-- <button
					id="register-button"
					type="button"
					class="btn btn-black btn-sm w-100 mt-1"
					onclick="change_register_page()"
				>
					Não tem cadastro?
				</button> 
			</div>

			<div class="col-sm-10 mx-auto bottom-align" id="register-page-buttons">
				<button type="button" class="btn btn-black btn-sm w-100 mb-1" onclick="register_request()">
					Registrar
				</button>
				<button
					id="register-button"
					type="button"
					class="btn btn-black btn-sm w-100 mt-1"
					onclick="change_login_page()"
				>
					Já tem cadastro?
				</button>
			</div>

			<div class="col-sm-10 mx-auto bottom-align" id="code-page-buttons">
				<button type="button" class="btn btn-black btn-sm w-100 mb-1" onclick="register()">
					Finalizar Registro
				</button>
			</div>-->
			</div>
		</div>
	</div>
</section>
