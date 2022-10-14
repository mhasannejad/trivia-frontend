<script>
    import axios from "axios";
    import {baseUrl} from "../utils/consts.js";
    import {userD} from "../utils/auth.js";
    import {navigate} from "svelte-navigator";

    export let challenge

    const joinF = () => {
        navigate('/challenge/' + challenge.id)
    }
    let date = Date.now() - Date.parse(challenge.created_at)
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
            <b>{challenge.creator.email === $userD.email ? 'YOU' : challenge.creator.symbol_name}</b> created this
            challenge at {days} days and {hours} hours and {Math.round(remainOfHours)} mins<br>
            {#if challenge.joiner}
                <b>{challenge.joiner.email === $userD.email ? 'YOU' : challenge.joiner.symbol_name}</b> joined
            {/if}
        </p>
        {#if challenge.creator.email === $userD.email && challenge.private}
            <br>
            <p>invitation code (share this to your opponent): <code>{challenge.invitation_code}</code></p>
        {/if}
        {#if !$userD}
            <button class="sm-btn">
                create an account first
            </button>
        {:else}
            <button class="sm-btn" on:click={joinF}>
                continue
            </button>
        {/if}

    </div>
</div>
