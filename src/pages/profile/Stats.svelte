<script>
    import {onMount} from "svelte";
    import axios from "axios";
    import ProfileLayout from "../ProfileLayout.svelte";
    import {baseUrl} from "../../utils/consts.js";
    import {userD} from "../../utils/auth.js";


    let user_stats = {}
    let prescription_stats = {}
    onMount(() => {
        axios({
            url: `${baseUrl}api/users/profile/self/stats/`,
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
        axios({
            url: `${baseUrl}drugs/profile/stats/`,
            method: 'GET',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.token
            }
        }).then((r) => {
            if (r.status === 200) {
                prescription_stats = r.data
                console.log(r.data)
            }
        })
    })
</script>

<ProfileLayout>
    <div slot="title">
        <h3>Your Statistics</h3>
    </div>
    <div slot="content">
        <h4 class="text-white">
            Challenges
        </h4>
        <div class="row">
            <div class="col-lg-6 col-md-6 col-sm-6 text-center">
                <div class="stat-item">
                    <h2>{user_stats.completed_challenges}</h2>
                    <h4 class="text-white" >Completed Challenges</h4>
                </div>
            </div>
            <div class="col-lg-6 col-md-6 col-sm-6 text-center ">
                <div class="stat-item">
                    <h2>{user_stats.total_challenges}</h2>
                    <h4 class="text-white" >Total Challenges</h4>
                </div>
            </div>
            <div class="col-lg-4 col-md-6 col-sm-6 text-center">
                <div class="stat-item">
                    <h2>{user_stats.total_questions}</h2>
                    <h4 class="text-white" >Total Questions</h4>
                </div>
            </div>
            <div class="col-lg-4 col-md-6 col-sm-6 text-center">
                <div class="stat-item">
                    <h2>{user_stats.total_right}</h2>
                    <h4 class="text-white" >Total Right Answers</h4>
                </div>
            </div>
            <div class="col-lg-4 col-md-6 col-sm-6 text-center ">
                <div class="stat-item">
                    <h2>{user_stats.total_wrong}</h2>
                    <h4 class="text-white" >Total Wrong Answers</h4>
                </div>
            </div>
        </div>
        <h4 class="text-white">
            Prescriptions
        </h4>
        <div class="row">
            <div class="col-lg-6 col-md-6 col-sm-6 text-center">
                <div class="stat-item">
                    <h2>{prescription_stats.correct_prescribed}</h2>
                    <h4 class="text-white" >Correct Prescriptions</h4>
                </div>
            </div>
            <div class="col-lg-6 col-md-6 col-sm-6 text-center ">
                <div class="stat-item">
                    <h2>{prescription_stats.wrong_prescribed}</h2>
                    <h4 class="text-white" >Wrong Prescriptions</h4>
                </div>
            </div>
            <div class="col-lg-6 col-md-6 col-sm-6 text-center ">
                <div class="stat-item">
                    <h2>{prescription_stats.points_earned}</h2>
                    <h4 class="text-white" >Total Points</h4>
                </div>
            </div>
            <div class="col-lg-6 col-md-6 col-sm-6 text-center">
                <div class="stat-item">
                    <h2>{prescription_stats.total_prescriptions}</h2>
                    <h4 class="text-white" >Total Prescriptions</h4>
                </div>
            </div>

        </div>
    </div>
</ProfileLayout>
