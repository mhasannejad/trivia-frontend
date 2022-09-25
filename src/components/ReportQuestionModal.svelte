<script>
    import LoginWidget from "./LoginWidget.svelte";
    import {closeModal} from "svelte-modals";
    import axios from "axios";
    import {baseUrl} from "../utils/consts.js";
    import {userD} from "../utils/auth.js";
    import {navigate} from "svelte-navigator";

    export let isOpen
    export let questionId

    let reportObj = {
        is_good: false,
        comment: '',
        question_id: questionId,
    }

    const setIsGood = (isGood) => {
        reportObj.is_good = isGood
    }

    const submitReport = () => {
        axios({
            method: 'POST',
            url: `${baseUrl}api/question/report/`,
            headers: {
                'Content-Type': 'application/json',
                'Authorization': `Bearer ${$userD.token}`
            },
            data: JSON.stringify(reportObj)
        }).then(r => {
            if (r.status === 201) {
                closeModal()
            }
        })
    }
</script>
{#if isOpen}}
    <div role="dialog" class="modal">
        <div class="contents">
            <div class="mb-3">
                <label class="form-label">comment on this question</label>
                <textarea bind:value={reportObj.comment} class="form-control" rows="3"></textarea>
            </div>
            <div class="row">
                <div class="col-6 d-flex justify-content-center ">
                    <i class="bi {reportObj.is_good ? 'bi-hand-thumbs-up-fill' : 'bi-hand-thumbs-up'} fs-2"
                       on:click={()=>{setIsGood(true)}}></i>
                </div>
                <div class="col-6 d-flex justify-content-center">
                    <i class="bi  {reportObj.is_good ? 'bi-hand-thumbs-down' : 'bi-hand-thumbs-down-fill'} fs-2"
                       on:click={()=>{setIsGood(false)}}></i>
                </div>
            </div>
            <button on:click={submitReport}>submit</button>
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
        min-width: 450px;
        border-radius: 6px;
        padding: 16px;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        pointer-events: auto;
        background-color: white !important;

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
