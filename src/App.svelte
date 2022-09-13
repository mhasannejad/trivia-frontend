<script>
    import {SvelteToast} from '@zerodevx/svelte-toast';
    import {createHistory, createMemorySource, Link, Route, Router} from "svelte-navigator";
    import Home from "./pages/Home.svelte";
    import StartChallenge from "./pages/StartChallenge.svelte";
    import MainChallenge from "./pages/MainChallenge.svelte";
    import Results from "./pages/Results.svelte";
    import {closeModal, Modals} from "svelte-modals";
    import {
        Collapse,
        Navbar,
        NavbarToggler,
        NavbarBrand,
        Nav,
        NavItem,
        NavLink
    } from 'sveltestrap';

    const options = {}
    const memoryHistory = createHistory(createMemorySource());
</script>


<SvelteToast {options}/>


<Router>
    <Navbar color="#212121" dark={true}>
        <NavbarBrand href="/" class="me-auto">DrChallenge</NavbarBrand>
        <Nav>
            <NavItem>
                <Link class="link-light nav-link" to="/challenge/all">Challenges</Link>

            </NavItem>
            <NavItem  class="nav-item">
                <Link class="link-light nav-link" to="/results">Results</Link>
            </NavItem>
        </Nav>
    </Navbar>
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
