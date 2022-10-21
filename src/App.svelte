<script>
    import {SvelteToast} from '@zerodevx/svelte-toast';
    import {createHistory, createMemorySource, Link, navigate, Route, Router} from "svelte-navigator";
    import Home from "./pages/Home.svelte";
    import StartChallenge from "./pages/StartChallenge.svelte";
    import MainChallenge from "./pages/MainChallenge.svelte";
    import Results from "./pages/Results.svelte";
    import {closeModal, Modals, openModal} from "svelte-modals";
    import {
        Collapse,
        Navbar,
        NavbarToggler,
        NavbarBrand,
        Nav,
        NavItem,
        NavLink
    } from 'sveltestrap';
    import LabelPrescription from "./pages/LabelPrescription.svelte";
    import ModeratePrescription from "./pages/ModeratePrescription.svelte";
    import {userD} from "./utils/auth.js";
    import LoginModal from "./components/LoginModal.svelte";
    import RegisterModal from "./components/RegisterModal.svelte";
    import Profile from "./pages/Profile.svelte";
    import MyProfile from "./pages/ProfileLayout.svelte";
    import Stats from "./pages/profile/Stats.svelte";
    import Prescriptions from "./pages/profile/Prescriptions.svelte";

    const options = {}
    const memoryHistory = createHistory(createMemorySource());
</script>


<SvelteToast {options}/>


<Router>
    <Navbar color="#212121" dark={true}>
        <a on:click={()=>{
            navigate('/')
        }} class="nav-link link-light fs-3 m-3">DrChallenge</a>
        <Nav>
            <NavItem>
                <Link class="link-light nav-link" to="/challenge/all">Challenges</Link>

            </NavItem>
            <NavItem class="nav-item">
                <Link class="link-light nav-link" to="/results">Leaderboard</Link>
            </NavItem>
            <NavItem class="nav-item">
                <Link class="link-light nav-link" to="/mine/profile/stats">Your Profile</Link>
            </NavItem>
            {#if $userD.token}
                <NavItem class="nav-item">
                    <a class="link-light nav-link" on:click={()=>{navigate('/profile/'+$userD.id)}}>Share Card<i
                            class="bi bi-bar-chart-line-fill mx-2"></i></a>
                </NavItem>
                <NavItem class="nav-item">
                    <a class="link-light nav-link" on:click={()=>{$userD={}}}>log out ({$userD.symbol_name})<i
                            class="bi bi-box-arrow-right mx-2"></i></a>
                </NavItem>
            {:else}
                <NavItem class="nav-item">
                    <a class="link-light nav-link" on:click={()=>{openModal(LoginModal)}}>log in</a>
                </NavItem>
                <NavItem class="nav-item">
                    <a class="link-light nav-link" on:click={()=>{openModal(RegisterModal)}}>register</a>
                </NavItem>
            {/if}

        </Nav>
    </Navbar>
    <Route path="/">
        <Home/>
    </Route>
    {#if $userD.token}
        <Route path="challenge/all">
            <StartChallenge/>
        </Route>

        <Route path="challenge/:id" let:params>
            <MainChallenge id={params.id}/>
        </Route>
        <Route path="mine/profile/stats" >
            <Stats />
        </Route>
        <Route path="mine/profile/prescription" >
            <Prescriptions />
        </Route>
        <Route path="results">
            <Results/>
        </Route>
        <Route path="profile/:id" let:params>
            <Profile id={params.id}/>
        </Route>
        <Route path="/prescription/label">
            <LabelPrescription/>
        </Route>
    {/if}
    {#if $userD.is_valid_to_moderate === true}
        <Route path="/prescription/moderate">
            <ModeratePrescription/>
        </Route>
    {/if}
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
