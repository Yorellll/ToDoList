<script lang="ts">
    let tmp: string | null;
    let listeTodos: [string, unknown][];

    const getTodos = () => {
        tmp = localStorage.getItem("todosList");
        if (tmp) {
            listeTodos = Object.entries(JSON.parse(tmp));
            console.log(listeTodos);
        }
    }
</script>
<button on:click={getTodos}>Refresh</button>
<ul>
    {#if listeTodos}
        {#each listeTodos as todo}
            <!--{#if typeof todo[1] === "object"}-->
            <li>
                <h2>{todo[1]?.title}</h2>
                <button>Supprimer</button>
                <button>Archiver</button>
            </li>
            <br>
            <!--{/if}-->
        {/each}
    {/if}
</ul>