<script lang="ts">
    import type {typeListe} from "./FormListe.svelte";
    import App from "../App.svelte";
    // Le type des todos à été difini à la création, je le récupère juste
    let listeTodos: typeListe[];
    let listTask: String[];

    const getTodos = () => {
        // Pour récupérer les todos
        listeTodos = JSON.parse(localStorage.getItem("todosList") || "[]");
        // console.log(listeTodos);
    };

    // Déclenchenement de la fonction getTodos
    getTodos();

</script>

<!-- <button on:click={getTodos}>Refresh</button> -->
<section class="lists container">
    <h2 class="big-title">Listes actives</h2>
    <ul class="lists-content">
        {#if listeTodos}
            {#each listeTodos as todo}
                <!--        Le const est ici car avec une simple "variable" = " ", elle s'affiche dans le dom-->
                {@const callTask = listTask = todo["todos"]}
                <li class="list">
                    <a href="/{todo?.urlTitle}"><h3>{todo?.title}</h3></a>
                    <button>Supprimer</button>
                    <button>Archiver</button>
                    {#if (listTask[0])}
                        {#each {length: 2} as _, i}
                            <li>
                                <label for="did">{listTask[i]}</label>
                                <!-- Retirer checkbox et mettre image -->
                                <input type="checkbox" name="did">
                            </li>
                            {#if i === 1}
                                <li>...</li>
                            {/if}
                        {/each}
                    {/if}
                </li>
            {/each}
        {/if}

    </ul>
</section>