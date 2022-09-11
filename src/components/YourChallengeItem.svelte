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
</script>


<div class="card my-3 ">
    <div class="card-header">
        {challenge.subject.title}
    </div>
    <div class="card-body">
        <p>
            {challenge.creator.email} created this challenge at { Math.round(date) } minutes ago
        </p>

        {#if !$userD}
            <button class="sm-btn">
                create an account first
            </button>
        {:else}
            <button class="sm-btn" on:click={joinF}>
                countinue
            </button>
        {/if}

    </div>
</div>
