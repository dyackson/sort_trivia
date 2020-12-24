<svelte:head>
    <style src='../../scss-entry.scss'></style>
</svelte:head>

<script>
	import Nav from '../components/Nav.svelte';

	export let segment;
</script>

<style>
	main {
    max-width: 40em;
    position: relative;
    margin: 0 auto;
    padding-top: 2em;
}
</style>

<Nav {segment}/>

<main>
	<slot></slot>
</main>
