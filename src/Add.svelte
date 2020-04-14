<div>
    <h1>Add two numbers: {a} + {b}</h1>
    <p>What's the correct answer?</p>

    {#each options as option}
    <input type="button" value={option} on:click|preventDefault={validateAnswer}>
    {/each}
    
</div>

<script>
    import { onMount, beforeUpdate } from 'svelte';

    // initialize a and b as random ints between 1 and 10
    let a = Math.floor(Math.random() * 11);
    let b = Math.floor(Math.random() * 11);

    // set a and b to 1 if they are 0
    if (a == 0) a = 1;
    if (b == 0) b = 1;

    // calculate answer
    const answer = a + b;

    // calculate alternate options in same manner as a and b
    let alt1 = Math.floor(Math.random() * 11) + Math.floor(Math.random() * 11);
    let alt2 = Math.floor(Math.random() * 11) + Math.floor(Math.random() * 11);

    // check they are not 0
    if (alt1 == 0) alt1 = 1;
    if (alt2 == 0) alt2 = 1;

    // check that alternate options do not equal answer
    while (alt1 == answer || alt1 == alt2) {
        alt1 = Math.floor(Math.random() * 11) + Math.floor(Math.random() * 11);
        console.log("alt1 equalled answer so changing")
    }

    while (alt2 == answer || alt2 == alt1) {
        alt2 = Math.floor(Math.random() * 11) + Math.floor(Math.random() * 11);
        console.log("alt2 equalled answer so changing")
    }

    // form array for answer and alternate options
    let options = [answer, alt1, alt2];

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

    // shuffle the options array
    shuffle(options);

    console.log(`${a} + ${b} = ${answer}`);

    // validate answer function
    function validateAnswer(e) {
        if (e.target.value == answer) {
            alert('CORRECT');
        } else {
            alert("INCORRECT");
        }
    }

</script>