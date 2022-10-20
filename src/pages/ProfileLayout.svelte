<script>

import {navigate} from "svelte-navigator";
</script>
<div class="container">
    <div class="row ">
        <div class="col-md-3 col-sm-12">
            <ul class="list-group dark-list">
                <li class="list-group-item"
                on:click={()=>{navigate('/mine/profile/stats')}}>Stats</li>
                <li class="list-group-item"
                    on:click={()=>{navigate('/mine/profile/edit')}}>Edit Profile</li>
                <li class="list-group-item"
                    on:click={()=>{navigate('/mine/profile/prescription')}}>Prescription History</li>
                <li class="list-group-item">Wrong Questions</li>
                <li class="list-group-item">Logout</li>
            </ul>
        </div>
        <div class="col-md-9 col-sm-12">
            <div class="" style="margin-top: 150px">
                <h3>
                    <slot name="title">

                    </slot>
                </h3>
                <div>
                    <slot name="content">

                    </slot>
                </div>
            </div>
        </div>
    </div>
</div>
