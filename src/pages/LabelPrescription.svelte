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
            method: 'GET',
        }).then((r) => {
            drugs = r.data
            console.log(drugs)
        })
    })
    const extract = (item) => item.name;
    const extractDose = (item) => item.dose;
    let prescriptionFormObj = {
        name: '',
        drug: '',
        perTime: '',
        count: '',
        trading_name: '',

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

            subsets = r.data
            //forms = r.data['form_list']
        })

    }

    const getRandomPrescriptionToLabel = () => {
        axios({
            url: `${baseUrl}drugs/prescription/random/`,
            method: 'GET',
            timeout: 50000,
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
                trading_name: prescriptionFormObj.trading_name,
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

<div class="container " style="margin-top: 100px">
    <div class="row">
        <div class=" col-sm-12 col-md-6">
            <img class="img-fluid rounded-2" src={`${baseUrl}`+current_prescription.image_url} alt="sample prescription"
                 width="500px">
        </div>
        <div class="col-sm-12 col-md-6">
            <button on:click={getRandomPrescriptionToLabel}>
                next prescription
            </button>
            <div class="card  px-1">
                <div class="card-header">
                    add new drug
                </div>
                <div class="card-body">
                    <div class="input-group">
                        <Typeahead limit={15} label="drugs" data={drugs} {extract}
                                   on:select={({detail})=>{getDrugSubsets(detail.selected)}}/>
                        <select bind:value={prescriptionFormObj.drug} name="dose" id="dose" class="form-select"
                                aria-label="dose">
                            {#each subsets as subset,index}
                                <option value={subset}>{subset.dose} {subset.drug_form}</option>
                            {/each}
                        </select>


                    </div>
                    <div class="input-group">
                        <input bind:value={prescriptionFormObj.perTime} type="text" class="form-control"
                               placeholder="prescription order">
                        <input bind:value={prescriptionFormObj.count} type="text" class="form-control"
                               placeholder="count">
                        <input bind:value={prescriptionFormObj.trading_name} type="text" class="form-control"
                               placeholder="commercial name">
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
                            order: {prescription.perTime} <br>
                            count: {prescription.count} <br>
                            commercial name: {prescription.count} <br>
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
