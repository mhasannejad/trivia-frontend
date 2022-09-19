<script>
    import {onMount} from "svelte";
    import Typeahead from "svelte-typeahead";
    import axios from "axios";
    import {baseUrl} from "../utils/consts.js";
    import {userD} from "../utils/auth.js";
    import {toast} from "@zerodevx/svelte-toast";



    let current_prescription = {}
    let items = []
    onMount(() => {
        getRandomPrescriptionToModerate()

    })





    const getRandomPrescriptionToModerate = () => {
        axios({
            url: `${baseUrl}drugs/prescription/tomoderate/`,
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.token
            }
        }).then((r) => {
            current_prescription = r.data.prescription
            items = r.data.items
        })
    }
    const addVerification = (prescriptionitem_id,is_correct) => {
        axios({
            url: `${baseUrl}drugs/prescription/submit/verification/`,
            method: 'POST',
            data:JSON.stringify({
                prescription_id:prescriptionitem_id,
                is_correct:is_correct,
            }),
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.token
            }
        }).then((r) => {
            if (r.status===201){
                toast.push('submitted')
                getRandomPrescriptionToModerate()
            }
        })
    }
</script>

<div class="container">
    <div class="row">
        <div class="col-1"></div>
        <div class="col-4" >
            <img style="position: fixed !important" class="img-fluid rounded-2" src={`${baseUrl}`+current_prescription.image_url} alt="sample prescription"
                 width="500px">
        </div>
        <div class="col-1"></div>
        <div class="col-6">

            {#each items as item}
                <div class="card py-3 px-1 my-2">
                    <div class="card-header">
                        {item.pharmacist.symbol_name} prescribes this prescription as :
                    </div>
                    <div class="card-body">
                        {#each item.prescription_items as prescription}
                            <div class="card my-3">
                                <div class="card-header">
                                    {prescription.drug.drug.name}
                                </div>
                                <div class="card-body">
                                    <p>
                                        dose: {prescription.drug.dose} <br>
                                        form: {prescription.drug.drug_form} <br>
                                        per time: {prescription.per_time} <br>
                                        count: {prescription.count} <br>
                                    </p>
                                </div>
                                <div class="card-footer row d-flex justify-content-between">
                                    <button class="btn-sm btn-danger sm-btn col-md-5 mx-2" on:click={()=>{addVerification(prescription.id,true)}}>
                                        is correct
                                    </button>
                                    <button class="btn-sm btn-danger sm-btn col-md-5 mx-2" on:click={()=>{addVerification(prescription.id,false)}}>
                                        is BS
                                    </button>
                                </div>
                            </div>
                        {/each}
                    </div>


                </div>
            {/each}


        </div>
    </div>
</div>
