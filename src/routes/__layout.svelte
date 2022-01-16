<script>
	import Navbar from '../components/Navbar.svelte';
	import '../styles/global.css';
</script>

<Navbar/>
<div class="flex flex-grow items-center flex-col">
	<slot/>
</div>

<style>
</style>
