<script lang="ts">
	import '../assets/style/HomeSocial.css';
	import NavKanan from '../komponen/NavKananHome.svelte';
	import PostCard from '../komponen/PostCard.svelte';
	import NewUserContainer from '../komponen/NewUserContainer.svelte';

	let mode: 'login' | 'register' = 'login';
</script>

<section class="homeSocial">
	<NavKanan />

	<main class="postContainer">
		<PostCard />
	</main>

	<NewUserContainer />
</section>
