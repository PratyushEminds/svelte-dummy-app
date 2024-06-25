<script>
	import Component from "../component/Component.svelte";
</script>

<svelte:head>
	<title>Population app</title>
	<meta name="description" content="Population demo app" />
</svelte:head>

<section>
	<Component/>
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
