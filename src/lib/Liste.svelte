<script lang="ts">
  import type { typeListe } from "./FormList.svelte";
  // Le type des todos à été difini à la création, je le récupère juste
  let listeTodos: typeListe[];
  // Par défaut on cach les todos archivées
  let hide: boolean = true;

  // Pour savoir si il y a des todos archivées
  $: hasArchivedTodos = listeTodos.some((todo) => todo.archive === true);


  const getTodos = () => {
    // Pour récupérer les todos
    listeTodos = JSON.parse(localStorage.getItem("todosList") || "[]");
  };

  const archiveTodo = (todo: typeListe) => {
    const archive = todo.archive;
    todo.archive = !archive;

    localStorage.setItem("todosList", JSON.stringify(listeTodos));
    hide = false;

    getTodos();
  };

  // Pour afficher ou cacher les todos archivées via le bouton
  const toggleHideArchive = () => {
    hide = !hide;
  };

  // Pour afficher les todos au chargement de la page
  getTodos();
</script>

<section class="lists container">

  <div>
    <h2 class="big-title">Listes actives</h2>
    {#if hasArchivedTodos}
      {#if hide === true}
        <button class:active={hide} on:click={toggleHideArchive}>
          Afficher les listes archivées
        </button>
      {:else}
        <button class:active={hide} on:click={toggleHideArchive}>
          Cacher les listes archivées
        </button>
      {/if}
    {/if}
  </div>
  <ul class="lists-content" aria-labelledby="list">
    {#if listeTodos}
      {#each listeTodos as todo}
        {#if todo.archive !== true}
          <li class="list">
            <a aria-label={`Lien vers ${todo.title}`} href="/{todo?.urlTitle}">
              <h3>{todo?.title}</h3>
            </a>
            <button on:click={() => archiveTodo(todo)}>
              <img
                class="list-button"
                src="/src/assets/archive.svg"
                alt="Pour archiver"
              />
            </button>
            {#each { length: 2 } as _, i}
              {#if todo["todos"][i]}
                <div>
                  <label for="did">{todo["todos"][i]["task"]}</label>
                  <!-- Retirer checkbox et mettre image -->
                  <input type="checkbox" name="did" />
                </div>
                {#if i === 1}
                  <p>...</p>
                {/if}
              {/if}
            {/each}
          </li>
        {/if}
      {/each}
    {/if}
</section>

{#if hasArchivedTodos}
  <section class:hide class="lists archives container">
    <h3 class="big-title">Listes archivées</h3>
    <ul role="list" class="lists-content">
      {#each listeTodos as todo}
        {#if todo.archive === true}
          <li class="list">
            <a href="/{todo?.urlTitle}"><h3>{todo?.title}</h3></a>
            <button on:click={() => archiveTodo(todo)}>
              <img
                class="list-button"
                src="/src/assets/archive.svg"
                alt="Pour archiver"
              />
            </button>
          </li>
        {/if}
      {/each}
    </ul>
  </section>
{/if}
