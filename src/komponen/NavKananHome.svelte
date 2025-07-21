<script lang="ts">
	export let namaPengguna: string = '';
	export let bio: string = '';
	import '../assets/style/HomeSocial.css';

	const token = localStorage.getItem('token');

	async function logout() {
		if (!token) {
			console.error('Token tidak ditemukan');
			return;
		}

		try {
			const res = await fetch('http://localhost:8000/api/logout', {
				method: 'POST',
				headers: {
					Authorization: `Bearer ${token}`,
					'Content-Type': 'application/json'
				}
			});

			if (res.ok) {
				console.log('Logout berhasil');
				localStorage.removeItem('token');
				localStorage.removeItem('user_id');
				window.location.href = '/'; // arahkan ke halaman login
			} else {
				const err = await res.json();
				console.error('Gagal logout:', err);
			}
		} catch (e) {
			console.error('Error saat logout:', e);
		}
	}
</script>

<div class="navHome Kiri">
	<section class="profil">
		<div class="containerFotoProfil">
			<img src="/defaultProfile.png" class="profilUser" alt="" />
			<button class="editButton"><img src="/iconEdit.svg" alt="Edit" /></button>
		</div>

		<div class="userName">
			<button class="editButton"><img src="/iconEdit.svg" alt="Edit" /></button>
			<h3>{namaPengguna}</h3>
		</div>

		<div class="editBio">
			<button class="editButton"><img src="/iconEdit.svg" alt="Edit" /></button>
			<h4>{bio}</h4>
		</div>
	</section>

	<section class="buttonContainer">
		<button class="addPost">
			<img src="/iconUpload.svg" alt="Upload" />Tambah Postingan
		</button>
		<button class="logOut" on:click={logout}>
			<img src="/iconOut.svg" alt="Logout" />Keluar
		</button>
	</section>
</div>
