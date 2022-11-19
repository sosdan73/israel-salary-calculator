<script>
    import Card from './lib/Card.svelte'
    import TaxCard from "./lib/TaxCard.svelte";
    import Trademark from "./lib/Trademark.svelte";
</script>

<div class="app">
    <header class="app__header --padding">
        <h1 class="app__title">Israel Salary Calculator</h1>
    </header>
    <main class="app__main --padding d-grow">
        <Card>
            <TaxCard/>
        </Card>
    </main>
    <footer class="app__footer --padding">
        <span class="d-grow">
            Made by <Trademark/>
        </span>
        <span>2022 &copy;</span>
    </footer>
</div>

<style>
    .app {
        display: flex;
        flex-direction: column;
        min-height: 100vh;
        width: 100vw;
        overflow-x: hidden;
        font-family: Inter, Avenir, Helvetica, Arial, sans-serif;
        color-scheme: light dark;
        color: #222222;
    }
    .app > .--padding {
        padding-left: 20px;
        padding-right: 20px;
    }
    .app__header,
    .app__footer {
        display: flex;
        background-color: var(--dark);
        color: var(--light);
    }
    .app__header {
        padding-top: 10px;
        padding-bottom: 10px;
    }
    .app__title {
        margin-top: 0;
        margin-bottom: 0;
        font-size: 1.8rem;

    }
    .app__main {
        display: flex;
        justify-content: center;
        align-items: center;
        background-color: var(--light);
    }
    .app__footer {
        padding-top: 20px;
        padding-bottom: 20px;
    }
</style>
