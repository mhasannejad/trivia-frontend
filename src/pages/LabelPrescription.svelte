<script>
    import {onMount} from "svelte";
    import Typeahead from "svelte-typeahead";
    import axios from "axios";
    import {baseUrl} from "../utils/consts.js";
    import {userD} from "../utils/auth.js";


    let prescriptions = []
    let drugs = []
    let subsets = []
    let forms = []
    let current_prescription = {}
    onMount(() => {
        getRandomPrescriptionToLabel()
        axios({
            url: `${baseUrl}drugs/drug/list/`,
            method: 'GEt',
        }).then((r) => {
            drugs = r.data
        })
    })
    const extract = (item) => item.name;
    const extractDose = (item) => item.dose;
    let prescriptionFormObj = {
        name: '',
        drug: '',
        perTime: '',
        count: '',

    }


    const getDrugSubsets = async (drugname) => {
        prescriptionFormObj.name = drugname
        console.log('gere')
        subsets = []
        forms = []
        await axios({
            url: `${baseUrl}drugs/drug/subset/${drugname}/`,
            method: 'GET',
        }).then((r) => {
            console.log('gere2')
            subsets = r.data
            //forms = r.data['form_list']
        })

    }

    const getRandomPrescriptionToLabel = () => {
        axios({
            url: `${baseUrl}drugs/prescription/random/`,
            method: 'GET',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.token
            }
        }).then((r) => {
            current_prescription = r.data
            prescriptions = []
            prescriptionFormObj = {
                name: '',
                drug: '',
                perTime: '',
                count: '',
            }
        })
    }
    const addPresitemToPrescription = () => {
        axios({
            url: `${baseUrl}drugs/prescription/submit/drug/`,
            method: 'POST',
            data: JSON.stringify({
                prescription_id: current_prescription.id,
                drugsubset_id: prescriptionFormObj.drug.id,
                per_time: prescriptionFormObj.perTime,
                count: prescriptionFormObj.count,
            }),
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + $userD.token
            }
        }).then((r) => {
            prescriptions = [{...prescriptionFormObj}, ...prescriptions]
            console.log(prescriptions)
            for (const member in prescriptionFormObj) prescriptionFormObj[member] = '';
        })
    }
</script>

<div class="container mt-5">
    <div class="row">
        <div class="col-1 col-md-1"></div>
        <div class="col-4 col-sm-12 col-md-4">
            <img class="img-fluid rounded-2" src={`${baseUrl}`+current_prescription.image_url} alt="sample prescription"
                 width="500px">
        </div>
        <div class="col-1 col-md-1"></div>
        <div class="col-6 col-sm-12 col-md-6">
            <button on:click={getRandomPrescriptionToLabel}>
                next prescription
            </button>
            <div class="card  px-1">
                <div class="card-header">
                    add new drug
                </div>
                <div class="card-body">
                    <div class="input-group">
                        <Typeahead label="drugs" data={drugs} {extract}
                                   on:select={({detail})=>{getDrugSubsets(detail.selected)}}/>
                        <select bind:value={prescriptionFormObj.drug} name="dose" id="dose" class="form-select"
                                aria-label="dose">
                            {#each subsets as subset,index}
                                <option value={subset}>{subset.dose} {subset.drug_form}</option>
                            {/each}
                        </select>

                        <input bind:value={prescriptionFormObj.perTime} type="text" class="form-control"
                               placeholder="per time?">
                        <input bind:value={prescriptionFormObj.count} type="text" class="form-control"
                               placeholder="count">
                    </div>
                </div>

                <div class="card-footer">
                    <button class="sm-btn" on:click={addPresitemToPrescription}>
                        save
                    </button>
                </div>
            </div>
            {#each prescriptions as prescription}
                <div class="card my-3">
                    <div class="card-header">
                        {prescription.name}
                    </div>
                    <div class="card-body">
                        <p>
                            dose: {prescription.drug.dose} <br>
                            form: {prescription.drug.drug_form} <br>
                            per time: {prescription.perTime} <br>
                            count: {prescription.count} <br>
                        </p>
                    </div>
                    <!--<div class="card-footer">
                        <button class="btn-sm btn-danger sm-btn">
                            delete
                        </button>
                    </div>-->
                </div>
            {/each}

        </div>
    </div>
</div>
