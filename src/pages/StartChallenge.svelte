<script>
    import ChallengeItem from "../components/ChallengeItem.svelte";
    import {userD} from "../utils/auth.js";
    import LoginWidget from "../components/LoginWidget.svelte";
    import RegisterWidget from "../components/RegisterWidget.svelte";
    import CreateChallengeWidget from "../components/CreateChallengeWidget.svelte";
    import {onMount} from "svelte";
    import axios from "axios";
    import {baseUrl} from "../utils/consts.js";
    import YourChallengeItem from "../components/YourChallengeItem.svelte";
    import JoinChallengeByCode from "../components/JoinChallengeByCode.svelte";

    let challenges = []
    let userChallenges = []
    onMount(() => {
        refresh()
        if ($userD.token) {
            getUserChallenges()
        }
    })

    const refresh = () => {
        if (!$userD.token) {
            axios({
                url: `${baseUrl}api/challenge/available/`,
                method: 'GET',
            }).then(r => {
                challenges = r.data
            })
        } else {
            axios({
                url: `${baseUrl}api/challenge/available/foruser/`,
                method: 'GET',
                headers: {
                    'Content-Type': 'application/json',
                    'Authorization': 'Bearer ' + $userD.token
                }
            }).then(r => {
                challenges = r.data
            })
        }
    }


    const getUserChallenges = () => {
        axios({
            url: `${baseUrl}api/challenge/mine/`,
            method: 'GET',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.token
            }
        }).then(r => {
            userChallenges = r.data
        })
    }
</script>

<div class="container">
    <div class="row">
        <div class="col-lg-1"></div>
        <div class="col-lg-5">
            {#if !$userD.token}
                <LoginWidget/>
                <RegisterWidget/>
            {:else }
                <h4 class="white-header">your challenges (not completed)</h4>
                {#each userChallenges as i}
                    <YourChallengeItem challenge={i}/>
                {/each}
            {/if}

        </div>
        <div class="col-lg-5">
            {#if $userD.token}
                <JoinChallengeByCode />
                <h4 class="white-header">
                    or
                </h4>
                <CreateChallengeWidget on:created={()=>{
                    refresh()
                    getUserChallenges()
                }}/>
            {/if}

            <h4 class="white-header">Available challenges</h4>
            {#each challenges as i}
                <ChallengeItem challenge={i}/>
            {/each}
        </div>
        <div class="col-lg-1"></div>
    </div>
</div>
