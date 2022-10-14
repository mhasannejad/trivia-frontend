<script>
    import axios from "axios";
    import {baseUrl} from "../utils/consts.js";
    import {userD} from "../utils/auth.js";
    import {navigate} from "svelte-navigator";
    import {toast} from "@zerodevx/svelte-toast";

    export let challenge

    const joinF = () => {
        if($userD.token){
            axios({
                url: `${baseUrl}api/challenge/join/`,
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                    'Authorization': 'Bearer ' + $userD.token
                }, data: JSON.stringify({
                    'challenge_id': challenge.id
                })
            }).then(r => {
                if (r.status === 202) {
                    navigate('/challenge/' + challenge.id)
                }
            })
        }else {
            toast.push('login please')
        }
    }
    let date = NaN
    date = Date.now() - Date.parse(challenge.created_at)
    date = date / 1000 / 60
    console.log(challenge.created_at)
    let days = Math.round(date / (60 * 24))

    let remainOfDay = date % (60 * 24)

    let hours = Math.round(remainOfDay / 60)

    let remainOfHours = remainOfDay % 60
</script>


<div class="card my-3 ">
    <div class="card-header">
        {challenge.subject.title}
    </div>
    <div class="card-body">
        <p>
            <b>{challenge.creator.symbol_name}</b> created this challenge at {days} days and {hours} hours and {Math.round(remainOfHours)} mins ago
        </p>

        {#if !$userD}
            <button class="sm-btn">
                create an account first
            </button>
        {:else}
            <button class="sm-btn" on:click={joinF}>
                join
            </button>
        {/if}

    </div>
</div>
