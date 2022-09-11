<script>
    import {SvelteToast} from '@zerodevx/svelte-toast';
    import {Route, Router} from "svelte-navigator";
    import Home from "./pages/Home.svelte";
    import StartChallenge from "./pages/StartChallenge.svelte";
    import MainChallenge from "./pages/MainChallenge.svelte";
    import Results from "./pages/Results.svelte";
    import {closeModal, Modals} from "svelte-modals";

    const options = {}

</script>


<SvelteToast {options}/>
<Router>
    <Route path="/">
        <Home/>
    </Route>
    <Route path="challenge/all">
        <StartChallenge/>
    </Route>

    <Route path="challenge/:id" let:params>
        <MainChallenge id={params.id}/>
    </Route>

    <Route path="results">
        <Results/>
    </Route>
</Router>

<Modals>
    <div
            slot="backdrop"
            class="backdrop"
            on:click={closeModal}
    ></div>
</Modals>


<style>
    .backdrop {
        position: fixed;
        top: 0;
        bottom: 0;
        right: 0;
        left: 0;
        background: rgba(0, 0, 0, 0.50)
    }
</style>
