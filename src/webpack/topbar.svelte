<script lang="js">
  let { page = '' } = $props();

  const pages = [
    { "name": "Home", "url": "/"},
    { "name": "Wiki", "url": "/wiki"},
    { "name": "News", "url": "/news"},
    { "name": "Shares", "url": "/shares"},
  ]
</script>

<main>
    <div class="topbar">
        <div class="child">
            <div class="buttons">
                {#each pages as _page}
                    <a href={_page.url} class:active={page === _page.name.toLowerCase()}>{_page.name}</a>
                {/each}
            </div>
        </div>
    </div>
</main>

<style>
    .topbar {
        position: fixed;
        width: 100%;
        z-index: 1000;

        display: flex;
        justify-content: center;

        .child {
            display: flex;
            align-items: center;
            justify-content: center;
            margin-top: 10px;

            padding: 0.85rem 1rem;
            background-color: rgba(0, 0, 0, 0.15);
            backdrop-filter: blur(10px);
            width: 30rem;
            border-radius: 20px;
        }

        .buttons {
            display: flex;
            gap: 0.85rem;
            @media screen and (max-width: 768px) { overflow-x: auto; }

            a {
                color: white;
                text-decoration: none;
                padding: 0.75rem 1.5rem;
                scale: 1;
                transition: all 0.2s;
                border-radius: 20px;
                background-color: rgba(40, 40, 40, 0.25);

                border-top: 2px solid var(--border);
                border-bottom: 2px solid transparent;
            } a:hover {
                scale: 1.1;

                border-top: 2px solid transparent;
                border-bottom: 2px solid var(--border);
            } a.active {
                background-color: rgba(40, 40, 40, 0.75);
            }
        }
    }
</style>
