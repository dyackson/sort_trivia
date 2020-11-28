<svelte:head>
    <style src='../../scss-entry.scss'></style>
</svelte:head>

<script>
	import Nav from '../components/Nav.svelte';

	export let segment;
</script>

<style>
	main {
	}
</style>

<Nav {segment}/>

<main>
	<slot></slot>
</main>
