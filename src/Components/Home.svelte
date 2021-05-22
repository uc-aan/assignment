<script>
	import App from '../App.svelte';
	import Endtest from './Endtest.svelte';
	import Questions from './Questions.svelte';
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

// 	function handleMessage(event) {
//         dispatch('onCancle', {
//         text: 'Hello!'
//         });
// }

	let confirmValue = 'home';

	function reStart() {
		confirmValue = 'dashboard';
	}
</script>

<main>
	{#if confirmValue !== 'home' }
		<App />
	{:else}
	<Endtest on:reStart={reStart} />
	{/if}
</main>