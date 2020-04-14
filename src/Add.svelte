<div class="outer">

    <input type="button" value="Play again?" id="replay-btn" on:click|preventDefault={replayGame} />

    <h1>{prompt}</h1>

    <GridBox cards={a} />
    <div>
        <img src="/images/plus-sign.png" alt="plus-sign" />
    </div>
    <GridBox cards={b} />

    <section>
        {#each options as option}
            <input class="answer-btn" type="button" value={option} on:click|preventDefault={validateAnswer}>
        {/each}
    </section>
    
</div>

<style>
    .outer {
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        height: 100%;
        background-color: lightslategrey;
        border-radius: 6px;
    }

    #replay-btn {
        position: fixed;
        right: 1rem;
        display: none;
        font-size: 1.5rem;
        font-family: 'Comic Sans MS';
        border-radius: 8px;
        color: lightgoldenrodyellow;
        background-color: darkslategray;
    }

    .answer-btn {
        width: 6rem;
        font-size: 1.5rem;
        font-family: 'Comic Sans MS';
        border-radius: 8px;
        color: lightgoldenrodyellow;
        background-color: darkslategray;
    }

    h1 {
        /* border: solid black 2px; */
        text-align: center;
        color:lightgoldenrodyellow;
        margin-bottom: 0rem;
    }

    div {
        display: flex;
        justify-content: center;
    }

    img {
        height: 40px;
        width: 40px;
    }

    section {
        /* border: solid black 2px; */
        display: flex;
        justify-content: space-evenly;
        margin-bottom: 1rem;
    }
</style>

<script>
    import { onMount, tick, beforeUpdate } from 'svelte';
    import GridBox from './GridBox.svelte';

    let prompt = "Add up the bananas!"

    // initialize a and b as random ints between 1 and 10
    let a = Math.floor(Math.random() * 11);
    let b = Math.floor(Math.random() * 11);

    // calculate alternate options in same manner as a and b
    let alt1 = Math.floor(Math.random() * 11) + Math.floor(Math.random() * 11);
    let alt2 = Math.floor(Math.random() * 11) + Math.floor(Math.random() * 11);

    // calculate answer
    $: answer = a + b;

    // form array for answer and alternate options
    $: options = [answer, alt1, alt2];

    $: console.log(`${a} + ${b} = ${answer}`);

    // function to shuffle an array
    // https://javascript.info/task/shuffle
    function shuffle(array) {
        for (let i = array.length - 1; i > 0; i--) {
            let j = Math.floor(Math.random() * (i + 1)); // random index from 0 to i

            // swap elements array[i] and array[j]
            // we use "destructuring assignment" syntax to achieve that
            // you'll find more details about that syntax in later chapters
            // same can be written as:
            // let t = array[i]; array[i] = array[j]; array[j] = t
            [array[i], array[j]] = [array[j], array[i]];
        }
    }

    // validate answer function
    function validateAnswer(e) {
        if (e.target.value == answer) {
            prompt = `${e.target.value} is correct!`;

            // change replay btn display to block to reveal it to user
            const replayBtn = document.getElementById('replay-btn');
            replayBtn.style.display = 'block';

            // disable answer buttons
            const answerBtnsCollection = document.getElementsByClassName('answer-btn');
            const answerBtns = [...answerBtnsCollection];
            answerBtns.forEach(btn => {
                btn.setAttribute('disabled', true);
            });
        } else {
            prompt = `${e.target.value} is incorrect - how many bananas are there?`;
        }
    }

    // function to validate game data
    async function validateGame() {
        console.log('validating game')
        // set a and b to 1 if they are 0
        if (a == 0) a = 1;
        if (b == 0) b = 1;

        // check they are not 0
        if (alt1 == 0) alt1 = 1;
        if (alt2 == 0) alt2 = 1;

        await tick();

        // check that alternate options do not equal answer
        while (alt1 == answer || alt1 == alt2) {
            console.log(`alt1: ${alt1} alt2: ${alt2} answer: ${answer} - updating alt1`)
            alt1 = Math.floor(Math.random() * 11) + Math.floor(Math.random() * 11);
        }

        await tick();

        while (alt2 == answer || alt2 == alt1) {
            console.log(`alt2: ${alt2} alt1: ${alt1} answer: ${answer} - updating alt2`)
            alt2 = Math.floor(Math.random() * 11) + Math.floor(Math.random() * 11);
        }

        await tick();

    }

    function replayGame() {
        // reset prompt
        prompt = 'Add up the bananas!';

        // change replay btn display to none
        const replayBtn = document.getElementById('replay-btn');
        replayBtn.style.display = 'none';

        // re-enable answer buttons
        const answerBtnsCollection = document.getElementsByClassName('answer-btn');
        const answerBtns = [...answerBtnsCollection];
        answerBtns.forEach(btn => {
            btn.removeAttribute('disabled');
        });

        // recalculate a b alt1 and alt2
        // initialize a and b as random ints between 1 and 10
        a = Math.floor(Math.random() * 11);
        b = Math.floor(Math.random() * 11);

        // calculate alternate options in same manner as a and b
        alt1 = Math.floor(Math.random() * 11) + Math.floor(Math.random() * 11);
        alt2 = Math.floor(Math.random() * 11) + Math.floor(Math.random() * 11);

        validateGame();
    }

    beforeUpdate(() => {
        shuffle(options);
    });

    onMount(() => {
        validateGame();
    })

</script>