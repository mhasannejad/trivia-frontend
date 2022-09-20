<script>
    import {onMount} from "svelte";
    import axios from "axios";
    import {baseUrl} from "../utils/consts.js";
    import {userD} from "../utils/auth.js";
    import crown from '../assets/crown.png'

    export let id

    let user_stats = {}
    onMount(() => {
        axios({
            url: `${baseUrl}api/users/profile/${id}/`,
            method: 'GET',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.token
            }
        }).then((r) => {
            if (r.status === 200) {
                user_stats = r.data
                console.log(r.data)
            }
        })
    })
</script>
{#if user_stats.stats}
    <div class="container">
        <div class="row text-center">
            <div class="col-4"></div>
            <div class="col-4  col-sm-12">
                <div class="profile-container  text-center">

                    <img src={crown} alt="" width="200px" height="150px"> <br>
                    <h2 class="pb-2 pt-5">{user_stats.symbol_name}</h2>
                    <h3 class="py-2">ranking: {user_stats.ranking} / {user_stats.total_users}</h3>
                    <h3 class="py-2">total point: {user_stats.points} </h3>
                    <h3 class="py-1">wins: {user_stats.stats.wins} </h3>
                    <h3 class="py-1">losses: {user_stats.stats.losses} </h3>
                    <h3 class="py-1">draws: {user_stats.stats.draws} </h3>

                </div>
            </div>
            <div class="col-4"></div>
        </div>
    </div>
{/if}
