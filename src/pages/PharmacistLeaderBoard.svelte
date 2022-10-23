<script>
    import {onMount} from "svelte";
    import axios from "axios";
    import {baseUrl} from "../utils/consts.js";

    let users = []
    onMount(() => {
        axios({
            url: `${baseUrl}drugs/ranking/`,
            method: 'GET',
        }).then((r) => {
            users = r.data
            console.log(users)
        })
    })
</script>

<div class="container">
    <div class="text-center my-5">
        <h3>Pharmacist Leaderboard</h3>
    </div>
    <div class="row">
        <div class="col-3"></div>
        <div class="col-6">
            {#each users as user,index}
                <div class="card my-2">
                    <div class="card-header">
                        <h4 style="text-align: center">
                            #{index + 1} | {user.symbol_name}
                        </h4>
                    </div>
                    <div class="card-body">
                        points: {user.total_prescription_point} <br>

                        corrects: {user.correct_prescriptions_prescribed_len} <br>
                        wrongs: {user.wrong_prescriptions_prescribed_len} <br>
                    </div>

                </div>
            {/each}
        </div>
        <div class="col-3"></div>
    </div>
</div>
