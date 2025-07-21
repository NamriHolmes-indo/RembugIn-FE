<script lang="ts">
	import '../assets/style/HomeSocial.css';
	import NavKanan from '../komponen/NavKananHome.svelte';
	import PostCard from '../komponen/PostCard.svelte';
	import NewUserContainer from '../komponen/NewUserContainer.svelte';

	let mode: 'login' | 'register' = 'login';
	const token = localStorage.getItem('token');
	console.log('Token:', token);

	let profile: any = null;
	let errorMsg: string | null = null;

	$: namaPengguna = profile?.nama_lengkap || '';
	$: bio = profile?.bio || '';

	async function getProfile() {
		if (!token) {
			errorMsg = 'Token tidak tersedia di localStorage';
			return;
		}

		try {
			const res = await fetch('http://localhost:8000/api/wewayangan', {
				method: 'GET',
				headers: {
					Authorization: `Bearer ${token}`,
					'Content-Type': 'application/json'
				}
			});

			if (!res.ok) {
				throw new Error(`Error ${res.status}: ${res.statusText}`);
			}

			const json = await res.json();
			profile = json.data;
		} catch (err) {
			console.error('Gagal mengambil profil:', err);
			errorMsg = 'Gagal mengambil profil';
		}
	}

	getProfile();
</script>

<section class="homeSocial">
	<NavKanan {namaPengguna} {bio} />

	<main class="postContainer">
		<PostCard />
	</main>

	<NewUserContainer />
</section>
