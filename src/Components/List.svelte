<script>
    import { listSection } from '../store/list-store';
    import { onDestroy } from 'svelte';
    import { qReffrence } from '../store/questionRefference';
    

    let questionpointer;
    qReffrence.subscribe((data) => {
        questionpointer = data;
    })

    $:console.log(questionpointer);
    
    // let QAlist=[{id:1},{id:2},{id:3},{id:4},{id:5},{id:6},{id:7},{id:8},{id:9},{id:10}];

    export let QA = [];

    export let showSide;

    const unsubscribe = listSection.subscribe((data) => {
        showSide = data;
    });
    
    onDestroy(unsubscribe);

    function closeSide() {
        listSection.update((value) => {
            return false;
        });
    }
    
    function changeNumber(i) {
        qReffrence.update((value) => {
            value = i;
            return value;
        });
    }

</script>
 
<div class="sidepanel">
    <h1>Question's List</h1>
    <hr />

    <ul class="unordered_list">
        {#each QA as arrList ,i (arrList.id)}
        <li class="list_item">
            <div class="list_content" on:click={changeNumber(arrList.id)}>
                <p>{arrList.question}</p>
            </div>
        </li>
        {/each}
    </ul> 
        
    <!-- <ul class="unordered_list">
        <li class="list_item">
            <div class="list_content" on:click={changeNumberOne}>
                <input type="checkbox" disabled />
                {QA[0].question}
            </div>
        </li>
        <li class="list_item">
            <div class="list_content" on:click={changeNumberTwo}>
                <input type="checkbox" disabled />
                {QA[1].question}
            </div>
        </li>
        <li class="list_item">
            <div class="list_content" on:click={changeNumberThree}>
                <input type="checkbox" disabled />
                {QA[2].question}
            </div>
        </li>
        <li class="list_item">
            <div class="list_content" on:click={changeNumberFour}>
                <input type="checkbox" disabled />
                {QA[3].question}
            </div>
        </li>
        <li class="list_item">
            <div class="list_content" on:click={changeNumberFive}>
                <input type="checkbox" disabled />
                {QA[4].question}
            </div>
        </li>
        <li class="list_item">
            <div class="list_content" on:click={changeNumberSix}>
                <input type="checkbox" disabled />
                {QA[5].question}
            </div>
        </li>
        <li class="list_item">
            <div class="list_content" on:click={changeNumberSeven}>
                <input type="checkbox" disabled />
                {QA[6].question}
            </div>
        </li>
        <li class="list_item">
            <div class="list_content" on:click={changeNumberEight}>
                <input type="checkbox" disabled />
                {QA[7].question}
            </div>
        </li>
        <li class="list_item">
            <div class="list_content" on:click={changeNumberNine}>
                <input type="checkbox" disabled />
                {QA[8].question}
            </div>
        </li>
        <li class="list_item">
            <div class="list_content" on:click={changeNumberTen}>
                <input type="checkbox" disabled />
                {QA[9].question}
            </div>
        </li>
    </ul> -->
    
</div>

<style>
    *{
        font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
    }
    .sidepanel {
        margin-top: 3.5rem;
        display: block;
        padding: 1rem;
        width: 30%;
        height: 85vh;
        background: white;
        border-radius: 5px;
        z-index: 10;
        box-shadow: 0 2px 8px rgba(0,0,0,0.26);
        overflow-y: scroll;
    }

    ul {
        background-color: rgba(0,0,0,0.7);
        padding: 0;
        width: 100%;
    }
    li {
        padding: 0.5rem;
        margin: auto;
        font-size: 15px;
        list-style-type: none;
        color: whitesmoke;
    }
    /* .checkbox {
    height: 25px;
    width: 25px;
    margin-top: 7px;
    border:1px solid #000000;
    border-radius: 2px;
    } */
    /* .checked {
    height: 25px;
    width: 25px;
    margin-top: 7px;
    border:1px solid #000000;
    border-radius: 2px;
    } */
</style>