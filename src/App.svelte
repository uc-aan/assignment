<script>
	import Header from './UI/Header.svelte';
	import Questions from './Components/Questions.svelte';
	import Confirmexit from './Components/Confirmexit.svelte';
import Popup from './Components/Popup.svelte';

	let closeTest = false;
	function exitTest(){
		closeTest = true;
		alert("hbff")
	}

</script>

<svelte:head>
  <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
</svelte:head>

<Header />
<Questions />
