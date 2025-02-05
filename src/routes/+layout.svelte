<script lang="ts">
	import '../app.css';
	import * as Card from '$lib/components/ui/card';
	import { onMount } from 'svelte';
	let { children } = $props();
	import pb from '$lib';
	let waiting = $state(true);
	let votes = $state(0);
	onMount(async () => {
		try {
			const petition = await pb.collection('petition').getList(1, 0);
			votes = petition.totalItems;
			waiting = false;
		} catch (error: any) {
			window.location.assign('/error?code=0');
			return;
		}
	});
</script>

<main class="dark flex text-center">
	<Card.Root class="m-auto inline-block h-[27rem] w-96 rounded-3xl border-none px-6">
		<Card.Header class="mb-2">
			<Card.Title class="text-2xl font-extrabold">Choice at Lunch!</Card.Title>
			{#if !waiting}
				<Card.Description
					><span class="font-extrabold text-green-500">{votes} people</span> have joined us.
					<a href="/reasons" class="font-bold text-blue-500 underline">Our Reasons</a
					></Card.Description
				>
			{/if}
		</Card.Header>
		{#if waiting}
			<p>Please Wait...</p>
		{:else}
			{@render children()}
		{/if}
	</Card.Root>
</main>

<style>
	main {
		height: 100vh;
		width: 100vw;
		/*background-image: conic-gradient(#fff, #080, #3c82f6, #fff);*/
		background-image: url('/background.jpg');
		background-size: cover;
	}
</style>
