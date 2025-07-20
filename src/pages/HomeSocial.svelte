<script lang="ts">
	import '../assets/style/HomeSocial.css';
	import NavKanan from '../komponen/NavKananHome.svelte';
	import PostCard from '../komponen/PostCard.svelte';
	import RegisterForm from '../komponen/RegisterForm.svelte';

	let mode: 'login' | 'register' = 'login';
</script>

<section class="homeSocial">
	<NavKanan />

	<main class="postContainer">
		<PostCard />
	</main>
</section>
