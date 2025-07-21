<script lang="ts">
	import { onMount } from 'svelte';
	import { writable } from 'svelte/store';
	import Gerbang from '../pages/Gerbang.svelte';
	import HomeSocial from '../pages/HomeSocial.svelte';

	const isLoggedIn = writable(false);

	async function checkTokenValidity() {
		const token = localStorage.getItem('token');

		if (!token) {
			isLoggedIn.set(false);
			return;
		}

		try {
			const res = await fetch('http://localhost:8000/api/check-token', {
				method: 'GET',
				headers: {
					Authorization: `Bearer ${token}`,
					'Content-Type': 'application/json'
				}
			});

			if (res.ok) {
				isLoggedIn.set(true);
			} else {
				isLoggedIn.set(false);
				localStorage.removeItem('token');
				localStorage.removeItem('user_id');
			}
		} catch (error) {
			console.error('Token check failed', error);
			console.log('error');
			isLoggedIn.set(false);
		}
	}

	onMount(() => {
		checkTokenValidity();
		console.log('HALO DUNIA');
	});

	async function handleLoginSuccess() {
		await checkTokenValidity();
	}
</script>

{#if $isLoggedIn}
	<HomeSocial />
{:else}
	<Gerbang on:loginSuccess={handleLoginSuccess} />
{/if}
