<script lang="ts">
	import '../assets/style/gerbang.css';
	import LoginForm from '../komponen/LoginForm.svelte';
	import RegisterForm from '../komponen/RegisterForm.svelte';

	let mode: 'login' | 'register' = 'login';
</script>

<div class="gerbang">
	<section class="gambarContainer"></section>

	<section class="formContainer">
		<h1>RembugIn</h1>

		<div class="toggle">
			<button on:click={() => (mode = 'login')} disabled={mode === 'login'}>Login</button>
			<button on:click={() => (mode = 'register')} disabled={mode === 'register'}>Register</button>
		</div>

		{#if mode === 'login'}
			<LoginForm />
		{:else}
			<RegisterForm />
		{/if}
	</section>
</div>
