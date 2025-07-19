<script lang="ts">
  let name = "";
  let email = "";
  let password = "";

  function handleRegister() {
    alert(`Register: ${name} - ${email}`);
  }
</script>

<form on:submit|preventDefault={handleRegister}>
  <h2>Register</h2>
  <input type="text" bind:value={name} placeholder="Nama Lengkap" required />
  <input type="email" bind:value={email} placeholder="Email" required />
  <input
    type="password"
    bind:value={password}
    placeholder="Password"
    required
  />
  <button type="submit">Daftar</button>
</form>
