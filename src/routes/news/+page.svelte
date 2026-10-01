<script lang="js">
    import { onMount } from 'svelte';
    import { marked } from 'marked';
    import { markedHighlight } from 'marked-highlight';
    import hljs from 'highlight.js';
    import 'highlight.js/styles/github-dark.css';
    import Topbar from '../../webpack/topbar.svelte';

    marked.use(markedHighlight({
        langPrefix: 'hljs language-',
        highlight(code, lang) {
            const language = hljs.getLanguage(lang) ? lang : 'plaintext';
            return hljs.highlight(code, { language }).value;
        }
    }));

    const page = 'news';
    
    let loadGetNews = $state(true);
    let errorGetNews = $state(false);
    let loadNewsFile = $state(false);
    let errorNewsFile = $state(false);
    
    let newsMarkdown = $state(''); 
    let newsList = $state([]);
    let currentNews = $state('');

    async function getNews() {
        try {
            const response = await fetch("/api/getmarkdown?type=news&act=getMarkdowns");
            if (!response.ok) throw new Error();
            return await response.json();
        } catch {
            errorGetNews = true;
            return [];
        } finally {
            loadGetNews = false;
        }
    }

    async function getNewsFile(file) {
        loadNewsFile = true;
        errorNewsFile = false;
        try {
            const response = await fetch(`/api/getmarkdown?type=news&act=getMarkdownFile&f=${encodeURIComponent(file)}`);
            if (!response.ok) throw new Error();
            return await response.json();
        } catch {
            errorNewsFile = true;
            return { content: `Failed to load article.` };
        } finally {
            loadNewsFile = false;
        }
    }

    async function selectNews(file, redir) {
        const data = await getNewsFile(file);
        currentNews = file;
        newsMarkdown = data.content;

        if (redir) {
            window.location.href = `/news?file=${encodeURIComponent(file.replace(".md", ""))}`;
        }
    }
    
    onMount(async () => {
        newsList = await getNews();

        const urlParams = new URLSearchParams(window.location.search);
        const newsParam = urlParams.get('file');

        if (newsParam) {
            await selectNews(newsParam + ".md", false);
        }
    });
</script>

<main class="page">
    <Topbar page={page}/>

    <div class="main">
        <div class="sidebar">
            <h3>News <span class="small">scrollable...</span></h3>
            {#if loadGetNews}
                <p>Loading...</p>
            {:else if errorGetNews}
                <p>Error loading news.</p>
            {:else}
                <div class="fileList">
                    {#each newsList as article}
                        <button class:active={currentNews === article.file} class="fileButton" onclick={() => selectNews(article.file, true)}>
                            {article.file.replace('.md', '')}
                        </button>
                    {/each}
                </div>
            {/if}
        </div>

        <div class="content">
            {#if loadNewsFile}
                <p>Loading content...</p>
            {:else if errorNewsFile}
                <p>Could not load content.</p>
            {:else if newsMarkdown}
                <article class="prose">
                    {@html marked.parse(newsMarkdown)}
                </article>
            {:else}
                <p>Select one of the few news we have. <br/> Just note, the example code looks to be broken, don\'t blame us!</p>
            {/if}
        </div>
    </div>
</main>

<style>
    .page {
        display: flex;
        flex-direction: column;
        height: 100vh;
    }

    .main {
        display: flex;
        flex: 1;
        min-height: 0;
        overflow: hidden;
        padding: 10px;
        @media screen and (max-width: 768px) { flex-direction: column; padding-top: 75px; }

        .sidebar {
            min-width: 250px;
            padding: 15px;
            overflow-y: auto;
            background-color: rgba(255, 255, 255, 0.025);
            border-top: 2px solid rgba(255, 255, 255, 0.1);
            border-radius: 20px;

            h3 { margin-top: 0; }

            .fileList {
                display: flex;
                flex-direction: column;
                gap: 8px;
                max-height: 150px;

                .fileButton {
                    background: none;
                    border: 1px solid transparent;
                    color: inherit;
                    text-align: left;
                    padding: 8px;
                    cursor: pointer;
                    transition: 0.2s;
                    border-radius: 10px;
                    font-family: funkin;

                    &:hover {
                        background: rgba(0, 0, 0, 0.2);
                    }
                }

                .active {
                    border-left: 2px solid white;
                    border-right: 2px solid white;
                }
            }
        }

        .content {
            flex: 3;
            padding: 20px;
            overflow-y: auto;
            @media screen and (min-width: 768px) { padding-top: 75px; }

            .prose {
                overflow-wrap: break-word;
                :global(h1) { font-size: 2rem; margin-bottom: 1rem; }
                :global(p) { margin-bottom: 1rem; line-height: 1.6; }
                :global(code) { background: rgba(0, 0, 0, 0.5); border-radius: 4px; }
                :global(a) { color: aqua; }
                :global(hr) { opacity: 0.1; }
                :global(img) { @media screen and (max-width: 768px) { width: 100%; } }
                :global(pre) {
                    background: rgba(0, 0, 0, 0.5);
                    border-radius: 4px;
                    padding: 15px;
                    margin-bottom: 1rem;
                    overflow-x: auto;
                    white-space: pre;
                }
                :global(pre code) {
                    background: none;
                    padding: 0;
                    border-radius: 0;
                }
                :global(h1, h2, h3, h4, h5, h6) {
                    border-bottom: 1px solid rgba(255,255, 255, 0.1);
                    padding-bottom: 0.5rem;
                }
            }
        }
    }
</style>
