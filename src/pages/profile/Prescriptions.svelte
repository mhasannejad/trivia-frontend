<script>
    import {onMount} from "svelte";
    import axios from "axios";
    import ProfileLayout from "../ProfileLayout.svelte";
    import {baseUrl} from "../../utils/consts.js";
    import {userD} from "../../utils/auth.js";


    let prescription_data = {
        wrong_prescriptions: []
    }
    onMount(() => {
        axios({
            url: `${baseUrl}drugs/profile/mine/`,
            method: 'GET',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.token
            }
        }).then((r) => {
            if (r.status === 200) {
                prescription_data = r.data
                console.log(prescription_data.wrong_prescriptions)
            }
        })
    })
</script>

<ProfileLayout>
    <div slot="title">
        <h3>Your Prescription History</h3>
    </div>
    <div slot="content">
        <h4 class="text-white fs-5">Wrong prescriptions (rated below 80%)</h4>
        <div class="row">

            {#each prescription_data.wrong_prescriptions as prescription}
                <div class="col-6 my-1">
                    <div class="card h-100 m-1">
                        <img src={baseUrl+prescription.prescription.image_url} class="card-img-top" alt="...">
                        <div class="card-body">
                            {#each prescription.presitems as presitem}
                                <div class="profile-presitem">
                                    <p>{presitem.drug.drug.name} | {presitem.drug.dose} | {presitem.drug.drug_form} </p>
                                    <p>order: {presitem.per_time} </p>
                                    <p>count: {presitem.count} </p>
                                    <p>commercial name: {presitem.trading_name} </p>
                                </div>
                            {/each}
                        </div>
                    </div>
                </div>
            {/each}
        </div>
    </div>
</ProfileLayout>
