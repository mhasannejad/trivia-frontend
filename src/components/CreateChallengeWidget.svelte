<script>
    import {createEventDispatcher, onMount} from "svelte";
    import axios from "axios";
    import {baseUrl} from "../utils/consts.js";
    import {userD} from "../utils/auth.js";
    import {navigate} from "svelte-navigator";

    let dispatcher = createEventDispatcher()

    let subjects = []
    let selectedSubject = NaN
    onMount(() => {
        axios({
            url: `${baseUrl}api/subjects/all/`,
            method: 'GET',
        }).then(r => {
            subjects = r.data
        })
    })


    const submitChallenge = () => {
        axios({
            method: 'POST',
            url: `${baseUrl}api/challenge/create/`,
            headers: {
                'Content-Type': 'application/json',
                'Authorization': `Bearer ${$userD.token}`
            },
            data: JSON.stringify({
                subject_id: selectedSubject
            })
        }).then(r => {
            if (r.status === 201) {
                navigate('/challenge/'+r.data.id)
            }
        })
    }
</script>

<div class="card my-3 ">
    <div class="card-header">
        Create New Challenge
    </div>
    <div class="card-body">
        <div class="mb-3">
            <select bind:value={selectedSubject} class="form-select" aria-label="Default select example">
                {#each subjects as subject, index}
                    <option value={subject.id}>{subject.title}</option>
                {/each}
            </select>
        </div>

        <button class="sm-btn" on:click={submitChallenge}>
            submit
        </button>
    </div>

</div>
