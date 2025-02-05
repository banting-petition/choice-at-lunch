<script lang="ts">
	import { browser } from '$app/environment';
	import { page } from '$app/state';
	let petition: any;
	if (browser) {
		petition = localStorage.getItem('petition');
	}
	const errors = [
		'There was a problem with the database. Please try again later.',
		'Incorrect Principal'
	];
</script>

<div>
	<p>{errors[Number(page.url.searchParams.get('code') || '0')]}</p>
</div>
