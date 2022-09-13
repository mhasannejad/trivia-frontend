<script>
    import axios from "axios";
    import {baseUrl} from "../utils/consts.js";
    import {userD} from "../utils/auth.js";
    import {navigate} from "svelte-navigator";

    let invitation_code = ''
    const joinChallenge = () => {
        axios({
            method: 'POST',
            url: `${baseUrl}api/challenge/join/invitation/`,
            headers: {
                'Content-Type': 'application/json',
                'Authorization': `Bearer ${$userD.token}`
            },
            data: JSON.stringify({
                invitation_code: invitation_code,
            })
        }).then(r => {
            if (r.status === 202) {
                navigate('/challenge/' + r.data.id)
            }
        })
    }
</script>

<div class="card my-3 ">
    <div class="card-header">
        Join Challenge by invitation code
    </div>
    <div class="card-body">
        <div class="mb-3">
            <label class="form-label">Invitation Code From MF who whats you dead</label>
            <input bind:value={invitation_code} type="text" class="form-control"
                   placeholder="**********">
        </div>

        <button class="sm-btn" on:click={joinChallenge}>
            join
        </button>
    </div>

</div>
