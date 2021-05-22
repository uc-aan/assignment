<script>
    import Header from '../UI/Header.svelte';
    import { qReffrence } from '../store/questionRefference';
    import { onDestroy } from 'svelte';
    import Popup from './Popup.svelte';
    import Timer from '../Components/Timer.svelte';
    import List from './List.svelte';
    import { reviewStatus } from '../store/review';
    import { listSection } from '../store/list-store';

    let qNumber;
    let startValue = 1;    
    
    let QA = [
		{
            id: 1,
			"question": "Who invented JavaScript?",
			"answers": [ 
                "Douglas Crockford",
                "Sheryl Sandberg",
                "Brendan Eich"
			],
			"correctAnswer": 3,
            "status": false
		},
		{
            id: 2,
			"question": "Which one of these is a JavaScript package manager?",
			"answers": [
                "Node.js",
                "TypeScript",
                "npm"
			],
			"correctAnswer": 3,
            "status": true
		},
		{
            "id": 3,
			"question": "Which tool can you use to ensure code quality?",
			"answers": [
                "Angular",
                "jQuery",
                "RequireJS",
                "ESLint"
			],
			"correctAnswer": 4,
            "status": false
		},
		{
            "id": 4,
			"question": "Which of the following are themselves a collection of different data types?",
			"answers": [
                "string",
                "structures",
                "char",
                "all of the mentioned"
			],
			"correctAnswer": 2,
            "status": false
		},
		{
            "id": 5,
			"question": "Which of the following cannot be a structure member?",
			"answers": [
                "Another structure",
                "Function",
                "Array",
                "None of the mentioned"
			],
			"correctAnswer": 2,
            "status": false
		},
		{
            "id": 6,
			"question": "User-defined data type can be derived by___________",
			"answers": [
                "struct",
                "enum",
                "typedef",
                "all of the mentioned"
			],
			"correctAnswer": 4,
            "status": false
		},
		{
            "id": 7,
			"question": "Which of the following is not a pointer declaration?",
			"answers": [
                "char a[10];",
                "char a[] = {‘1’, ‘2’, ‘3’, ‘4’};",
                "char *str;",
                "char a;"
			],
			"correctAnswer": 4,
            "status": false
		},
		{
            "id": 8,
			"question": "A variable declared in a function can be used in main().",
			"answers": [
                "True",
                "False",
                "True if it is declared static",
                "None of the mentioned;"
			],
			"correctAnswer": 2,
            "status": false
		},
		{
            "id": 9,
			"question": "The name of the variable used in one function cannot be used in another function.",
			"answers": [
                "True",
                "False",
			],
			"correctAnswer": 2,
            "status": false
		},
		{
            "id": 10,
			"question": "The format identifier ‘%i’ is also used for _____ data type.",
			"answers": [
                "char",
                "int",
                "float",
                "double"
			],
			"correctAnswer": 2,
            "status": false
		}
	];
    
    let ans = new Array(QA.length).fill(null);

    let hideBox = false;
    function startTest() {
        hideBox = true;
        qReffrence.update((value) => {
            return 0;
        });
    }

    $: qReffrence.subscribe((value) => {
        qNumber = value;
    });

    let showStatus;
    const unsubscribeReview = reviewStatus.subscribe((data) => {
        showStatus = data;
    });

    onDestroy(unsubscribeReview);

    
    function selectAnswer(i) {
        ans[qNumber]=i+1;
    }

    let q=0;
    
    let nextQuestionId = QA[q].id;
    let nextQuestion = QA[q].question;
    let nextOption = QA[q].answers;
    let opStatus = QA[q].status;
    
    function Next(){
        startValue +=1;
        
        if (startValue >= QA.length) {
            startValue = QA.length;
            const button = document.getElementById("btnNext");
            button.disabled = true;
        }
        
        if(q == QA.length - 1){
            q = QA.length;
        }
        else{
            const btnprev = document.getElementById("btnPrev");
            btnprev.disabled = false;
            q++;
        }
        nextQuestionId = QA[q].id;
        nextQuestion = QA[q].question;
        nextOption = QA[q].answers;
        // opStatus = QA[q].status;

        // let radioStatus = document.getElementById("radiobtn");

        // if (opStatus === false && opStatus <= 0) {
        //     radioStatus = true;
        //     opStatus = radioStatus;
        // }
        // console.log(opStatus +" "+ nextQuestionId);
    }

    function Previous(){
        startValue -= 1;

        if (startValue < 2) {
            startValue = 1;
            const btnprev = document.getElementById("btnPrev");
            btnprev.disabled = true;
            
        }

        if(q == 0){
            q = 0;
        }
        else{
            const button = document.getElementById("btnNext");
            button.disabled = false;
            q--;
        }
        nextQuestionId = QA[q].id;
        nextQuestion = QA[q].question;
        nextOption = QA[q].answers;
    }

    let ShowList = false;
    function showList() {
        ShowList = !ShowList;
        return ShowList;
    }
    ShowList = showList();

    let ShowPopup = false;
    function showPopup(){
        
        ShowPopup = !ShowPopup;
        return ShowPopup;
    }

    //onDestroy(unsubscribe);
</script>


<div class="Appcontainer">
    {#if !ShowList}
        <List QA={QA}/>
    {:else if ShowPopup}
        <Popup />
    {/if}
    
    {#if qNumber == -1}
        <div class="start_screen" class:clicked={hideBox}>
            <button class="btn btn-outline-success startbtn" on:click={startTest}>START TEST</button>
        </div>
    {:else if !(qNumber > ans.length-1)}
    
        <div class="test_screen">
            <div class="test-box">
                <h2>
                    QUESTION {nextQuestionId} : {nextQuestion}
                </h2>
                <div class="option">
                    {#each nextOption as opt,i}
                        <ul>
                            <li>
                                <label class="container">
                                    <input type="radio" on:click={() => {ans[qNumber]}}  name="options" disabled={showStatus} checked={ans[qNumber]==null ? false : showStatus ? false : 'checked'} />{"    " + opt}
                                    <span class="checkmark"></span>
                                </label>
                            </li>
                        </ul>
                    {/each}
                </div>

                <!-- {#if showStatus}
                    <h2 class="{QA[qNumber].correctIndex == ans[qNumber] ? 'correct' : 'incorrect'}">
                    <span>YOUR ANSWER : {ans[qNumber]}</span>
                    </h2> -->
                <!-- {/if} -->


        </div>

                <div class="footersection">    
                    <footer>
                        <div class="Footer">
                            <Timer />
                            <button class="btn btn-outline-info" on:click="{showList}">List</button>
                            <button class="btn btn-outline-primary" on:click="{Previous}" disabled="true" id="btnPrev" >Previous</button>
                            <p>{startValue} of 10</p>
                            <button class="btn btn-outline-success" on:click="{Next}" id="btnNext" >Next</button>
                            <button class="btn btn-outline-danger" on:click="{showPopup}">End Test</button>
                        </div>
                    </footer>
                </div>
    
</div>


{/if}

    

</div>

<style>

.test-box{
    height: 60%;
}
    
.option ul li{
    background-color: #000000;
    width: 400px;
    padding: 5px 5px 5px 20px;
    border-radius: 25px;
    transition: all 0.5s;
}

.option ul{
    list-style-type: none;
    margin: 1rem 0 0 20rem;
    font-size: 1.5rem;
    color: #ffffff;
    font-weight: 300;
}


.test_screen h2{
    font-weight: 400;
    margin: 4rem 0 0 10rem;
    font-size:1.75rem;
    padding: 1rem;
}


    .test_screen{
        background-color: blanchedalmond;
        height: 85vh;
        margin-top: 3.5rem;
        width: 100%;
    }
    .Appcontainer{
        width: 100%;
        height: 95vh;
        display: flex;
        justify-content: center;
        align-items: center;       
    }
    .startbtn{
        width: 200px;
        height: 50px;
        border-radius: 10px;
        font-size: 23px;

    }

    .start_screen {
   display: flex;
   justify-content: center;
   align-items: center;
}

    .clicked {
    display: none;
}


/* Footer style */

    .footersection {
        margin-top: 8rem;
        width: 100%;
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
</style>