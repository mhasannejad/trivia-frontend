<script>
    import {Link} from "svelte-navigator";
</script>

<div class="container">
    <div class="row">

        <div class="col-lg-4"></div>
        <div class="col-lg-4 col-sm-12" style="margin-top: 15%">
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
            <Link to="/ranking">
                <button>
                    Ranking
                </button>
            </Link>
        </div>
        <div class="col-lg-4"></div>
    </div>
</div>
