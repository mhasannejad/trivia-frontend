<script>
    import {Link} from "svelte-navigator";
    import {userD} from "../utils/auth.js";
</script>

<div class="container">
    <div class="row">

        <div class="col-lg-4"></div>
        <div class="col-lg-4 col-sm-12" style="margin-top: 5%">
            <Link to="/challenge/all">
                <button>
                    Start New Challenge
                </button>
            </Link>
            <Link to="/results">
                <button>
                    Your Challenges
                </button>
            </Link>
            <Link to="/prescription/label">
                <button>
                    Label Prescriptions
                </button>
            </Link>
            {#if $userD.role === 1}
                <Link to="/prescription/moderate">
                    <button>
                        Check Prescriptions (moderators only)
                    </button>
                </Link>
            {/if}


        </div>
        <div class="col-lg-4"></div>
    </div>
</div>
