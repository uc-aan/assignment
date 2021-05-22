<script>
    import { qReffrence } from '../store/questionRefference.js'; 
    import { listSection } from '../store/list-store.js';
    import Timer from '../Components/Timer.svelte';
    import { reviewStatus } from '../store/review.js';
    import { onDestroy } from 'svelte';
    import Popup from '../Components/Popup.svelte'

    let showTimer = true;
    let showModal = false;
    let initialValue = 0;
    let activebtn;
    
    function handlePrevious() {
        qReffrence.update((value) => {
            //initialValue = value;
            return value-1 ;
        });
        initialValue-=1;
    }
    function handleNext() {
        qReffrence.update((value) => {
            //console.log(value);
            //initialValue = value;
            return value+1 ;
        });
       initialValue+=1;
       }
    function handleAside() {
        listSection.update((value) => {
            return true;
        });
    }
    function endTest() {
        showModal = !showModal;
        initialValue = 0;
        qReffrence.update((value) => {
            return 0;
        });
    }
    $: if(initialValue <= 1) {
        activebtn = true;
    }
    else {
        activebtn = false;
    }

    const unsubscribetimer= reviewStatus.subscribe((data) => {
        showTimer = data;
    });
    //this is done to avoid memory leakage
    onDestroy(unsubscribetimer);
</script>

{#if showModal}
    <Popup />
{/if}

<footer class="footer">
   <div class="button_set" class:without="{showTimer}">
    {#if !showTimer} 
    <Timer />
    {/if}
    <button type="button" on:click={handleAside}>List</button>
    <button type="button" caption="PREVIOUS" on:click={handlePrevious} btndisabled={activebtn}>Previous</button>
    <span>{initialValue = (initialValue < 10) ? '0' + initialValue : initialValue}</span> <span>OF</span> <span>10</span>
    <button type="button" on:click={handleNext}>Next</button>
    {#if !showTimer}
    <button type="button" on:click={endTest}>End Test</button>
    {/if}
   </div>
</footer>

<style>
/* .footer {
    position : fixed;
    left : 0;
    bottom : 0;
    width : 100%;
    height : 4rem;
    background-color: rgb(128, 0, 0);
}
.button_set {
    position : absolute;
    right : 30px;
    top : 2px;
}
span {
    color : #ffffff;
    margin : 0px 5px 0px 5px;
    font-size : 1.05rem;
}
.without {
    position : absolute;
    right : 30px;
    top : 10px;
} */
</style>















<!-- <script context="module">

let indirectPage = '';

export function endTest() {
    const confirmValue = confirm("Are you sure, want to end test");
    // console.log(confirmValue);

    if(confirmValue === true){
        indirectPage = 'HomePage';
    }
}

</script> -->

<!-- 
<script>

    import Questions from '../Components/Questions.svelte';
    import Timer from '../Components/Timer.svelte';
    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();

    function sayHello() {
        dispatch('endtest');
    }
    
</script>

<footer>
    <div class="Footer">
        <span>
            <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 0 24 24" width="24px" fill="#FFFFFF"><path d="M0 0h24v24H0z" fill="none"/><path d="M15 1H9v2h6V1zm-4 13h2V8h-2v6zm8.03-6.61l1.42-1.42c-.43-.51-.9-.99-1.41-1.41l-1.42 1.42C16.07 4.74 14.12 4 12 4c-4.97 0-9 4.03-9 9s4.02 9 9 9 9-4.03 9-9c0-2.12-.74-4.07-1.97-5.61zM12 20c-3.87 0-7-3.13-7-7s3.13-7 7-7 7 3.13 7 7-3.13 7-7 7z"/></svg>
            <Timer />
        </span>
        <button class="btn btn-outline-info">List</button>
        <button class="btn btn-outline-primary">Previous</button>
        <p>number of questions</p>
        <button class="btn btn-outline-success">Next</button>
        <button class="btn btn-outline-danger" on:click={sayHello}>End Test</button>
    </div>
</footer>


<style>

    *{
        font-family: 'Courier New', Courier, monospace;
    }

    span {
        color: white;
        margin-right: 4rem;
    }

    footer {
        background-color: black ;
        display: flex;
        justify-content: center;
        align-items: center;
        width: 100%;
        box-shadow: -1px -3px 4px -1px rgba(0,0,0,0.63);
    }
    .Footer {
        display: flex;
        justify-content: space-between;
        padding: 0.5em;
        margin: 0 auto;
    }
    button {
        font-weight: bolder;
        width: 150px;
        border-radius: 5px;
        margin-left: 1rem;
    }
    p {
        margin-left: 1rem;
        color: white;
    }

</style> -->