<script>
    import axios from "axios";
    import {baseUrl} from "../utils/consts.js";
    import {userD} from "../utils/auth.js";
    import {toast} from "@zerodevx/svelte-toast";
    import {createEventDispatcher} from "svelte";

    let loginObj = {
        email: '',
        password: ''
    }
    let eventDispatcher = createEventDispatcher()
    const loginF = async () => {
        try {
            await axios({
                url: `${baseUrl}api/auth/login/`,
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                data: JSON.stringify(loginObj)
            }).then(r => {
                if(r.status===200){
                    $userD = r.data

                    eventDispatcher('loggedin')
                }else{

                }
            })
        }catch (error){
            if(error.response.status===401){
                toast.push('credentials are wrong')
            }
        }
    }
</script>


<div class="card my-3 ">
    <div class="card-header">
        Login to Your Account
    </div>
    <div class="card-body">
        <div class="mb-3">
            <label for="exampleFormControlInput1" class="form-label">Email address</label>
            <input bind:value={loginObj.email} type="email" class="form-control"
                   id="exampleFormControlInput1" placeholder="name@example.com">
        </div>
        <div class="mb-3">
            <label for="exampleFormControlInput1" class="form-label">Password</label>
            <input bind:value={loginObj.password} type="password" class="form-control" id="password"
                   placeholder="********">
        </div>
        <button class="sm-btn" on:click={loginF}>
            Login
        </button>
    </div>

</div>
