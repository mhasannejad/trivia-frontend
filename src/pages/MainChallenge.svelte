<script>
    import {flip} from 'svelte/animate';
    import {onMount} from "svelte";
    import axios from "axios";
    import {baseUrl} from "../utils/consts.js";
    import {userD} from "../utils/auth.js";
    import {toast} from "@zerodevx/svelte-toast";

    export let id;
    let challenge = null
    let questions = []
    onMount(() => {
        axios({
            url: `${baseUrl}api/challenge/get/${id}/`,
            method: 'GET',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.token
            }
        }).then(r => {
            challenge = r.data
            questions = challenge.questions
            console.log(challenge)
        })
    })


    const submitAnswer = (question_id, answer_id) => {
        console.log('sending response')
        axios({
            method: 'POST',
            url: `${baseUrl}api/challenge/submit/`,
            headers: {
                'Content-Type': 'application/json',
                'Authorization': `Bearer ${$userD.token}`
            },
            data: JSON.stringify({
                challenge_id: challenge.id,
                question_id: question_id,
                option_id: answer_id,
            })
        }).then(r => {
            questions = questions.filter(i => i.id !== question_id)
            if (r.status === 202) {
                toast.push('submitted')
            }
        })
    }
</script>


{#if challenge}
    <div class="container">
        <div class="row">
            <div class="col-3"></div>
            <div class="col-6">
                <div style="height: 100px;padding-top: 50px">
                    <h2 style="text-align: center">
                        {challenge.creator.email} VS {challenge.joiner.email}
                    </h2>
                </div>

                {#each questions as question (question)}
                    <div class="card p-3 my-2" animate:flip>
                        <div class="card-title mb-3">
                            <p class="text-lg-center">
                                {question.text}
                            </p>
                        </div>
                        <div class="row">
                            {#each question.options as option}
                                <div class="col-6">
                                    <div class=" option" on:click={()=>{
                                        submitAnswer(question.id,option.id)
                                    }}>
                                        <p style="text-align: center">{option.text}</p>
                                    </div>
                                </div>
                            {/each}


                        </div>
                    </div>
                {/each}
            </div>
            <div class="col-3"></div>
        </div>
    </div>
{/if}
