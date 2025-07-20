<script lang="ts">
	let email = '';
	let password = '';

	function handleLogin() {
		alert(`Login dengan email: ${email}`);
	}
</script>

<form on:submit|preventDefault={handleLogin}>
	<h2>Login</h2>
	<input type="email" bind:value={email} placeholder="Email" required />
	<input type="password" bind:value={password} placeholder="Password" required />
	<button type="submit">Login</button>
	<a href="#">Lupa password?</a>
</form>
