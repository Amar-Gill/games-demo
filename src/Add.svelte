 <!-- position: fixed  -->
 <input type="button" value="Play again?" id="replay-btn" on:click|preventDefault={replayGame} />

 <h5><time>{t}</time></h5>

 <aside id='pop-up'>
     <h6>~ NICE WORK ~</h6>
     <img src='/images/pbjt.gif' alt='gif' />
 </aside>
 <!-- position != fixed  -->

<div>
    <h1>{prompt}</h1>

    <GridBox cards={a} />
    <section>
        <img src="/images/plus-sign.png" alt="plus-sign" />
    </section>
    <GridBox cards={b} />

    <footer>
        {#each options as option}
            <input class="answer-btn" type="button" value={option} on:click|preventDefault={validateAnswer}>
        {/each}
    </footer>
</div>

<style>
    div {
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

    h1, h5 {
        /* border: solid black 2px; */
        text-align: center;
        color:lightgoldenrodyellow;
        margin-bottom: 0rem;
    }

    h5 {
        position: fixed;
        font-weight: bold;
        left: 7rem;
    }

    section {
        display: flex;
        justify-content: center;
    }

    aside {
        position: fixed;
        display: flex;
        flex-direction: column;
        align-items: center;
        /* height: fit-content;
        width: fit-content; */
        right: 1rem;
        bottom: 1rem;
        border: solid darkslategray 2px;
        background-color: pink;
    }

    /* note: to adjust size of encouragement pop up just change aside img height */
    aside h6 {
        text-align: center;
        margin: 0;
        background-color: darkslategray;
        color: lightgoldenrodyellow;
        width: 100%;
    }

    aside img {
        /* just adjust height to change the pop up dimensions */
        height: 10rem;
        margin:0;
    }

    section img {
        height: 40px;
        width: 40px;
    }

    #pop-up {
        display: none;
    }

    footer {
        /* border: solid black 2px; */
        display: flex;
        justify-content: space-evenly;
        margin-bottom: 1rem;
    }
</style>

<script>
    import { onMount, tick, beforeUpdate } from 'svelte';
    import GridBox from './GridBox.svelte';

    // initialize game prompt and clock for user
    let prompt = "Add up the bananas!"

    // initialize a and b as random ints between 1 and 10
    let a = Math.floor(Math.random() * 11);
    let b = Math.floor(Math.random() * 11);

    // initialize game timer
    let t = 0;
    let interval = setInterval(() => { t += 1 }, 1000);

    // calculate alternate options in same manner as a and b
    let alt1 = Math.floor(Math.random() * 11) + Math.floor(Math.random() * 11);
    let alt2 = Math.floor(Math.random() * 11) + Math.floor(Math.random() * 11);

    // calculate answer
    $: answer = a + b;

    // form array for answer and alternate options
    $: options = [answer, alt1, alt2];

    $: console.log(`${a} + ${b} = ${answer}`);

    // function to shuffle an array
    // TODO - refactor into utils
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

            console.log(t);

            clearInterval(interval);

            // change replay btn display to block to reveal it to user
            const replayBtn = document.getElementById('replay-btn');
            replayBtn.style.display = 'block';

            // change pop-up display to flex to reveal and maintain img proportions
            const popUp = document.getElementById('pop-up');
            popUp.style.display = 'flex';

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
        // recalculate a and b if they are 0
        while (a == 0) a = Math.floor(Math.random() * 11);

        while (b == 0) b = Math.floor(Math.random() * 11);

        await tick();

        // check that alternate options do not equal answer or each other or 0
        while (alt1 == answer || alt1 == alt2 || alt1 == 0) {
            console.log(`alt1: ${alt1} alt2: ${alt2} answer: ${answer} - updating alt1`)
            alt1 = Math.floor(Math.random() * 11) + Math.floor(Math.random() * 11);
        }

        await tick();

        while (alt2 == answer || alt2 == alt1 || alt2 == 0) {
            console.log(`alt2: ${alt2} alt1: ${alt1} answer: ${answer} - updating alt2`)
            alt2 = Math.floor(Math.random() * 11) + Math.floor(Math.random() * 11);
        }

        await tick();

    }

    // TODO - refactor function as initialize game by first removing top level code
    function replayGame() {
        // reset prompt
        prompt = 'Add up the bananas!';

        // change replay btn display to none
        const replayBtn = document.getElementById('replay-btn');
        replayBtn.style.display = 'none';

        // change pop-up display to none to hide from user again
        const popUp = document.getElementById('pop-up');
        popUp.style.display = 'none';

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

        // re-initialize game timer
        t = 0;
        interval = setInterval(() => { t += 1 }, 1000);

        validateGame();
    }

    beforeUpdate(() => {
        shuffle(options);
    });

    onMount(() => {
        validateGame();
    })

</script>