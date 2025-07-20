<script lang="ts">
	let name = '';
	let username = '';
	let bio = '';
	let email = '';
	let password = '';
	let retypePassword = '';
	let tanggalLahir = '';
	let showPassword = false;
	let showRetypePassword = false;
	let error = '';

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

	$: isFormValid =
		name.trim() &&
		username.trim() &&
		email.trim() &&
		password.trim() &&
		retypePassword.trim() &&
		tanggalLahir &&
		validateEmail(email) &&
		validatePassword(password) &&
		password === retypePassword &&
		usia !== null &&
		usia >= 17;

	function handleRegister() {
		error = '';

		if (!validateEmail(email)) {
			error = "Email tidak valid. Harus mengandung '@' dan '.'.";
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

		alert(`Berhasil daftar!\nNama: ${name}\nUsername: ${username}\nEmail: ${email}`);
	}
</script>

<form on:submit|preventDefault={handleRegister}>
	<h2>Register</h2>

	{#if error}
		<p style="color: red">{error}</p>
	{/if}

	<input type="text" bind:value={name} placeholder="Nama Lengkap" required />
	<input type="text" bind:value={username} placeholder="Username" required />

	<p>{bio.length}/50 karakter</p>
	<input type="text" bind:value={bio} placeholder="Bio Singkat (maks 50 karakter)" maxlength="50" />

	{#if tanggalLahir}
		<p>Usia Anda: {usia} tahun</p>
		{#if usia !== null && usia < 17}
			<p style="color: orange">⚠️ Usia Anda belum cukup untuk mendaftar (minimal 17 tahun).</p>
		{/if}
	{/if}
	<input type="date" bind:value={tanggalLahir} required />

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

	<button class="tombolDafLog" type="submit" disabled={!isFormValid}>Daftar</button>
</form>
