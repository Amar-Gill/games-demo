<script>

</script>

<nav>
    <ul>
        <li>
            nav-item
        </li>
        <li>
            nav-item
        </li>
        <li>
            nav-item
        </li>
        <li>
            nav-item
        </li>

    </ul>

</nav>

<style>
    /* TODO - refactor for media queries */
    nav {
        position: fixed;
        background-color:darkslategrey;
        top: 0;
        width: 5rem;
        height: 100vh;
    }

    ul {
    list-style: none;
    padding: 0;
    margin: 0;
    display: flex;
    flex-direction: column;
    align-items: center;
    height: 100%;
    }

    li {
        width: 100%;
        color: lightgoldenrodyellow;
    }

    li:last-child {
        margin-top: auto;
    }
</style>