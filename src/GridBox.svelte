<script>
    // number of cards to draw in the grid
    export let cards;
</script>

<section>
    <!-- https://github.com/sveltejs/svelte/issues/2968 -->
    {#each {length: cards} as card}
        <div>placeholder</div>
    {/each}
</section>

<style>
    section {
        display: grid;
        gap: 1rem;
        grid-template-columns: repeat(auto-fill, minmax(240px, 1fr));
    }

    div {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        background: #353535;
        font-size: 2rem;
        color: #fff;
        box-shadow: rgba(3, 8, 20, 0.1) 0px 0.15rem 0.5rem, rgba(2, 8, 20, 0.1) 0px 0.075rem 0.175rem;
        height: 100%;
        width: 100%;
        border-radius: 4px;
        transition: all 500ms;
        overflow: hidden;

        background-size: cover;
        background-position: center;
        background-repeat: no-repeat;
    }
</style>
