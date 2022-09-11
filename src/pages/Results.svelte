<script>
    import SwitchButton from "../assets/SwitchButton.svelte";
    import {onMount} from "svelte";
    import axios from "axios";
    import {baseUrl} from "../utils/consts.js";
    import {userD} from "../utils/auth.js";
    import {openModal} from "svelte-modals";
    import ChallengeModal from "../components/ChallengeModal.svelte";
    import LoginWidget from "../components/LoginWidget.svelte";
    import RegisterWidget from "../components/RegisterWidget.svelte";
    import {toast} from "@zerodevx/svelte-toast";

    let mode = false; // if false : users results if true all users results

    let results = []
    let ranking = []
    onMount(() => {
        getUserResults()
        axios({
            url: `${baseUrl}api/users/ranking/`,
            method: 'GET',
        }).then(r => {
            ranking = r.data
        })
    })
    const getUserResults = () => {
        if($userD.token){
            axios({
                url: `${baseUrl}api/challenge/result/mine/`,
                method: 'GET',
                headers: {
                    'Content-Type': 'application/json',
                    'Authorization': 'Bearer ' + $userD.token
                }
            }).then(r => {
                results = r.data
            })
        }
    }

    const submitChallenge = (user) => {
        if($userD.token){
            openModal(ChallengeModal, { title: 'Challenge',joiner_id:user.id })
        }else{
            toast.push('login please')
        }
    }
</script>

<div class="container">
    <div class="row">
        <div class="col-1"></div>
        <div class="col-5 pt-5">
            <div style="">
                <span class="m-2">yours</span>
                <SwitchButton bind:checked={mode}/>
                <span class="m-2">all users</span>
            </div>


            {#if $userD.token}
                {#each results as result}
                    <div class="card pt-4 my-3">
                        <div class="card-title">
                            <h4 style="text-align: center">
                                {result.creator.email} vs {result.joiner.email}
                            </h4>

                            <h3 style="text-align: center">
                                {result.result.creator} - {result.result.joiner}
                            </h3>
                        </div>
                    </div>
                {/each}
                {:else}
                <LoginWidget />
                <RegisterWidget />
            {/if}
        </div>
        <div class="col-5">
            {#each ranking as user}
                <div class="card my-2">
                    <div class="card-header">
                        <h4 style="text-align: center">
                            {user.email}
                        </h4>
                    </div>
                    <div class="card-body">
                        wins: {user.stats.wins} <br>
                        looses: {user.stats.losses} <br>
                        total right questions: {user.points} <br>
                    </div>
                    <button class="sm-btn mx-2" on:click={()=>{
                        submitChallenge(user)
                    }}>
                        challenge this user
                    </button>
                </div>
            {/each}

        </div>
        <div class="col-1"></div>
    </div>
</div>
