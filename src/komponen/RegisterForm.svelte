<script lang="ts">
	let name = localStorage.getItem('name') || '';
	let username = localStorage.getItem('username') || '';
	let bio = localStorage.getItem('bio') || '';
	let email = localStorage.getItem('email') || '';
	let password = localStorage.getItem('password') || '';
	let retypePassword = localStorage.getItem('retypePassword') || '';
	let tanggalLahir = localStorage.getItem('tanggalLahir') || '';
	let showPassword = false;
	let showRetypePassword = false;
	let error = '';
	let success = '';
	let loading = false;

	$: localStorage.setItem('name', name);
	$: localStorage.setItem('username', username);
	$: localStorage.setItem('bio', bio);
	$: localStorage.setItem('email', email);
	$: localStorage.setItem('password', password);
	$: localStorage.setItem('retypePassword', retypePassword);
	$: localStorage.setItem('tanggalLahir', tanggalLahir);

	function validateEmail(email: string): boolean {
		return email.includes('@') && email.includes('.');
	}

	function validatePassword(password: string): boolean {
		const minLength = password.length >= 12;
		const hasUppercase = /[A-Z]/.test(password);
		const hasNumber = /[0-9]/.test(password);
		const hasSpecialChar = /[^A-Za-z0-9]/.test(password);
		return minLength && hasUppercase && hasNumber && hasSpecialChar;
	}

	function hitungUsia(tanggal: string): number {
		const today = new Date();
		const lahir = new Date(tanggal);
		let usia = today.getFullYear() - lahir.getFullYear();
		const m = today.getMonth() - lahir.getMonth();
		if (m < 0 || (m === 0 && today.getDate() < lahir.getDate())) {
			usia--;
		}
		return usia;
	}

	$: usia = tanggalLahir ? hitungUsia(tanggalLahir) : null;

	async function handleRegister() {
		error = '';
		success = '';

		if (!validateEmail(email)) {
			error = "Email tidak valid. Harus mengandung '@' dan '.'";
			return;
		}

		if (!validatePassword(password)) {
			error = 'Password harus minimal 12 karakter, ada huruf besar, angka, dan karakter khusus.';
			return;
		}

		if (password !== retypePassword) {
			error = 'Password dan Retype Password tidak cocok.';
			return;
		}

		if (usia !== null && usia < 17) {
			error = 'Usia harus minimal 17 tahun untuk mendaftar.';
			return;
		}

		loading = true;

		try {
			const response = await fetch('http://localhost:8000/api/register', {
				method: 'POST',
				headers: {
					'Content-Type': 'application/json'
				},
				body: JSON.stringify({
					username: username,
					password: password,
					nama_lengkap: name,
					bio: bio || 'Pengguna baru',
					url_foto: 'foto.jpg',
					tanggal_lahir: tanggalLahir
				})
			});

			const data = await response.json();

			if (!response.ok) {
				error = data?.message || 'Registrasi gagal.';
			} else {
				success = 'Registrasi berhasil!';

				// Hapus semua input & localStorage
				name = '';
				username = '';
				bio = '';
				email = '';
				tanggalLahir = '';
				localStorage.clear();
			}
		} catch (err) {
			error = 'Terjadi kesalahan koneksi.';
		} finally {
			loading = false;
		}
	}

	$: formLengkap = name && username && bio && email && password && retypePassword && tanggalLahir;
</script>

<form on:submit|preventDefault={handleRegister}>
	<h2>Register</h2>

	{#if error}
		<p style="color: red">{error}</p>
	{/if}
	{#if success}
		<p style="color: green">{success}</p>
	{/if}

	<input type="text" bind:value={name} placeholder="Nama Lengkap" required />
	<input type="text" bind:value={username} placeholder="Username" required />

	<p>{bio.length}/50 karakter</p>
	<input type="text" bind:value={bio} placeholder="Bio Singkat (maks 50 karakter)" maxlength="50" />

	<input type="date" bind:value={tanggalLahir} required />
	{#if tanggalLahir}
		<p>Usia Anda: {usia} tahun</p>
	{/if}

	<input type="email" bind:value={email} placeholder="Email" required />

	<div class="password-wrapper">
		<input
			type={showPassword ? 'text' : 'password'}
			bind:value={password}
			placeholder="Password"
			required
		/>
		<button type="button" class="toggle-icon" on:click={() => (showPassword = !showPassword)}>
			<p>{showPassword ? '🙈' : '👁️'}</p>
		</button>
	</div>

	<div class="password-wrapper">
		<input
			type={showRetypePassword ? 'text' : 'password'}
			bind:value={retypePassword}
			placeholder="Retype Password"
			required
		/>
		<button
			type="button"
			class="toggle-icon"
			on:click={() => (showRetypePassword = !showRetypePassword)}
		>
			<p>{showRetypePassword ? '🙈' : '👁️'}</p>
		</button>
	</div>

	<button
		class="tombolDafLog"
		type="submit"
		disabled={!formLengkap || (usia !== null && usia < 17) || loading}
	>
		{loading ? 'Mendaftar...' : 'Daftar'}
	</button>
</form>
