<script>
	import Counter from './Counter.svelte';
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="diga 5" />
</svelte:head>

<section>
	<Counter />
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	}
</style>
