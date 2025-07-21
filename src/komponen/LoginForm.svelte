<script lang="ts">
	import { onMount } from 'svelte';

	let email = '';
	let password = '';
	let error = '';
	let success = '';

	async function handleLogin() {
		error = '';
		success = '';

		try {
			const response = await fetch('http://localhost:8000/api/login', {
				method: 'POST',
				headers: {
					'Content-Type': 'application/json'
				},
				body: JSON.stringify({
					username: email,
					password: password
				})
			});

			const result = await response.json();

			if (!response.ok) {
				error = result.message || 'Login gagal.';
				return;
			}

			if (result.token) {
				localStorage.setItem('token', result.token);
				localStorage.setItem('user_id', result.user_id);
				success = 'Login berhasil! Selamat datang.';
				window.location.href = '/';
			}

			email = '';
			password = '';
		} catch (e) {
			error = 'Terjadi kesalahan saat login.';
			console.error(e);
		}
	}
</script>

<form on:submit|preventDefault={handleLogin}>
	<h2>Login</h2>

	{#if error}
		<p style="color: red;">{error}</p>
	{/if}
	{#if success}
		<p style="color: green;">{success}</p>
	{/if}

	<input type="text" bind:value={email} placeholder="Username" required />
	<input type="password" bind:value={password} placeholder="Password" required />
	<button type="submit" class="tombolDafLog">Login</button>
	<!-- <a href="#">Lupa password?</a> -->
</form>
