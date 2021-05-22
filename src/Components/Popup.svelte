<script>
    import { listSection } from '../store/list-store';
    import { fade, fly, scale } from 'svelte/transition';
    // import Confirmexit from './Confirmexit.svelte';
    import Questions from './Questions.svelte';
    import App from '../App.svelte';
    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();

    export let showSide;

    const unsubscribe = listSection.subscribe((data) => {
        showSide = data;
    });
    
    // onDestroy(unsubscribe);

    function closeSide() {
        listSection.update((value) => {
            return false;
        });
    }

    let ExitTest = false;
    function CoExit(){
        dispatch('exit');
    }

    let restartTest = false;
    function reStart(){
        restartTest = !restartTest;
    }

</script>

<!-- {#if isOpen } -->

<div transition:fly={{ y: 300 }} class="model" >
    <header>
        <h1 class="text-danger">Your Score</h1>
    </header>

    <div class="container-fluid mt-5">
        <div class="row">
            <div class="col-lg-4 col-md-4 col-12">
                <button class="btn-outline-danger" on:click="{CoExit}">CONFIRM</button>
            </div>
            <div class="col-lg-4 col-md-4 col-12">
                <button class="btn-outline-warning" on:click="{reStart}">RE-START</button>
            </div>
            <div class="col-lg-4 col-md-4 col-12">
                <button class="btn-outline-primary">Review</button>
            </div>
        </div>
    </div>
</div>
<!-- {/if} -->

<style>
    *{
        font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
    }
    button {
        border-radius: 15px;
        width: 100px;
        font-size: large;
    }
    header {
        border-bottom: 1px solid red;
        text-align: center;
    }
    /* .backdrop {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100vh;
        background: rgba(0,0,0,0.8);
        z-index: 10;
    } */
    .model {
        padding: 1rem;
        position: fixed;
        top: 25vh;
        left: 35%;
        width: 30%;
        height: 40vh;
        background: white;
        border-radius: 5px;
        z-index: 100;
        box-shadow: 0 2px 8px rgba(0,0,0,0.26);
        
    }
</style>