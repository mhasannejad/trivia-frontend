<script>
    import {closeModal} from 'svelte-modals'
    import axios from "axios";
    import {baseUrl} from "../utils/consts.js";
    import {userD} from "../utils/auth.js";
    import {onMount} from "svelte";
    import {navigate} from "svelte-navigator";

    // provided by Modals
    export let isOpen

    export let title
    export let joiner_id
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
    const createChallenge = () => {
        axios({
            method: 'POST',
            url: `${baseUrl}api/challenge/somedude/`,
            headers: {
                'Content-Type': 'application/json',
                'Authorization': `Bearer ${$userD.token}`
            },
            data: JSON.stringify({
                subject_id: selectedSubject,
                joiner_id: joiner_id
            })
        }).then(r => {
            if (r.status === 201) {
                navigate('/challenge/all')
            }
        })
    }
</script>

{#if isOpen}
    <div role="dialog" class="modal">
        <div class="contents">
            <h2>{title}</h2>
            <div class="mb-3">
                <select bind:value={selectedSubject} class="form-select" aria-label="Default select example">
                    {#each subjects as subject, index}
                        <option value={subject.id}>{subject.title}</option>
                    {/each}
                </select>
            </div>
            <div class="actions">
                <button on:click="{createChallenge}">create challenge</button>
            </div>
        </div>
    </div>
{/if}

<style>
    .modal {
        position: fixed;
        top: 0;
        bottom: 0;
        right: 0;
        left: 0;
        display: flex;
        justify-content: center;
        align-items: center;

        /* allow click-through to backdrop */
        pointer-events: none;
    }

    .contents {
        min-width: 240px;
        border-radius: 6px;
        padding: 16px;
        background: white;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        pointer-events: auto;
    }

    h2 {
        text-align: center;
        font-size: 24px;
        color: #212121;
    }

    p {
        text-align: center;
        margin-top: 16px;
    }

    .actions {
        margin-top: 32px;
        display: flex;
        justify-content: flex-end;
    }
</style>
