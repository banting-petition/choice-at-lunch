<script lang="ts">
	import { Button } from '$lib/components/ui/button';
	import { Label } from '$lib/components/ui/label';
	import { Input } from '$lib/components/ui/input';
	import * as Select from '$lib/components/ui/select';
	import * as Card from '$lib/components/ui/card';
	import pb from '$lib';
	import { onMount } from 'svelte';
	let emailOrName = $state('');
	let principal: any = $state('');
	let waiting = $state(false);
	onMount(() => {
		if (localStorage.getItem('petition')) {
			window.location.assign('/already');
		}
	});
	async function submit(event: Event) {
		event.preventDefault();
		waiting = true;
		if (principal.value !== 1) {
			window.location.assign('/error?code=1');
			return;
		}
		if (localStorage.getItem('petition')) {
			window.location.assign('/already');
			return;
		}
		try {
			await pb.collection('petition').create({ email_or_name: $state.snapshot(emailOrName) });
		} catch (error: any) {
			window.location.assign('/error?code=0');
			return;
		}
		localStorage.setItem('petition', $state.snapshot(emailOrName));
		waiting = false;
		window.location.assign('/finished');
		return;
	}
</script>

{#if waiting}
	<p>Please Wait...</p>
{:else}
	<p class="text-sm">
		At Banting Middle School, Outside Lunches are times when the students are forced to go outside
		for lunch 10 minutes after lunch started.<br />
		<span class="font-bold">This is a petition to get rid of Outside Lunches.</span>
	</p>

	<h2 class="mt-3 text-xl font-bold">Join the Petition</h2>

	<div class="text-left"><Label for="input" class="font-bold">School Email:</Label></div>
	<form onsubmit={submit}>
		<Input
			id="input"
			type={emailOrName.includes('@') ? 'email' : 'text'}
			bind:value={emailOrName}
			placeholder="093-example@sd43.bc.ca"
			class="rounded-lg border-gray-700"
			required
		/>
		<p class="mt-1 text-xs">This information may be made public.</p>
		<Label
			><div class="my-1 mt-2 text-left font-bold">Our Principal (Security Question)</div>
			<Select.Root bind:selected={principal} required>
				<Select.Trigger class="border-gray-700">
					<Select.Value placeholder="Mr. Trask" />
				</Select.Trigger>
				<Select.Content class="dark border-gray-600">
					<Select.Item value={0}>Mr. Kimura</Select.Item>
					<Select.Item value={1}>Mr. McCutcheon</Select.Item>
					<Select.Item value={2}>Mr. Wilson</Select.Item>
					<Select.Item value={3}>Mr. Campbell</Select.Item>
				</Select.Content>
			</Select.Root></Label
		>

		<Card.Footer class="mt-3 text-center">
			<Button class="m-auto" type="submit">Submit</Button>
		</Card.Footer>
	</form>
{/if}
