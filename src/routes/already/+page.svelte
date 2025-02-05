<script>
	import { browser } from '$app/environment';
	let petition = $state('');
	if (browser) {
		petition = localStorage.getItem('petition') || '';
	}
</script>

<p class="text-sm">
	At Banting Middle School, Outside Lunches are times when the students are forced to go outside for
	lunch 10 minutes after lunch started.<br />
	<span class="font-bold">This is a petition to get rid of Outside Lunches.</span>
</p>

<hr class="my-5" />

<p class="font-bold">
	You have already joined the petition {#if petition !== 'true'}with this email:
		<span class="text-amber-500">{petition}</span>{/if}
</p>
