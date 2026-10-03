<script>    
import Topbar from '../webpack/topbar.svelte';
const page = 'about engine';
</script>

<main>
    <Topbar page={page}/>

    <div class="main">
        Solar Engine is a from "scratch" engine built on top of Base 0.2.8 (The Newgrounds preview.)

        As of right now the only published version is a fork of Psych 0.6.3 that adds modcharting, as well as backports some features (like note rgb coloring) from 0.7+ <br>
        The only maintainer of the Fork is <a href="https://github.com/CharGoldenYT">CharGoldenYT</a>

        The new 0.2.8 based version will release as 1.0 and potentially be the last major update, with the rest of the updates focusing on small improvements and bugfixes.
    </div>
</main>

<style>
    .main {
        --margin_from_others: 10px;
        margin-bottom: 20px;
        padding: 10px;

        .mainContent {
            display: flex;
            flex-direction: column;
        }
    }

    .title {
        display: flex;
        flex-direction: column;
        gap: 10px;
        margin-bottom: 20px;

        .bigboi {
            font-size: 2rem;
        }
    }

    .background {
        background-color: rgba(0, 0, 0, 0.2);
        padding: 10px;
        border-radius: 20px;
        border-top: 1px solid var(--border);
    }
    .pad { padding: 10px; }
</style>