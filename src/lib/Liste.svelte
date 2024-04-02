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

  //Pour supprimer une liste
  const deleteList = (todo: typeListe) => {
      listeTodos.splice(listeTodos.indexOf(todo), 1)
      localStorage.setItem("todosList", JSON.stringify(listeTodos));
      getTodos();
  }
  // Pour afficher les todos au chargement de la page
  getTodos();
</script>

<section class="lists container">
  <div class="lists-title">
    <h2 class="big-title">Listes actives</h2>
    {#if hasArchivedTodos}
      {#if hide === true}
        <button
          aria-label="Pour cacher les listes archivées"
          class:active={hide}
          class="showList"
          on:click={toggleHideArchive}
        >
          <i><img src="/src/assets/show_icon.svg" alt="Afficher les listes archivées" /></i>
          Afficher les listes archivées
        </button>
      {:else}
        <button
          aria-label="Pour afficher les listes archivées"
          class:active={hide}
          class="showList"
          on:click={toggleHideArchive}
        >
          <i><img src="/src/assets/hide_icon.svg" alt="Cacher les listes archivées" /></i>
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
            <div class="list-action">
              <a aria-label={`Lien vers ${todo.title}`} href="/{todo.urlTitle}">
                <h3>{todo.title}</h3>
              </a>
              <button aria-label={`Pour archiver la liste ${todo.title}`} on:click={() => archiveTodo(todo)}>
                <img class="list-button" src="/src/assets/archive.svg" alt="Pour archiver" />
              </button>
            </div>
            {#if todo.todos.length !== 0}
              <a class="list-preview" href="/{todo.urlTitle}" aria-label={`Lien vers ${todo.title}`}>
                {#each { length: 2 } as _, i}
                  {#if todo["todos"][i]}
                    <p>
                      {todo["todos"][i]["task"]}
                    </p>
                    {#if i === 1}
                      <p>...</p>
                    {/if}
                  {/if}
                {/each}
              </a>
            {/if}
          </li>
        {/if}
      {/each}
    {/if}
  </ul>
</section>

{#if hasArchivedTodos}
  <section class:hide class="lists archives container">
    <h3 class="big-title">Listes archivées</h3>
    <ul role="list" class="lists-content">
      {#each listeTodos as todo}
        {#if todo.archive === true}
          <li class="list">
            <div class="list-action">
              <a href="/{todo.urlTitle}"><h3>{todo.title}</h3></a>
              <button on:click={() => archiveTodo(todo)}>
                <img class="list-button" src="/src/assets/archive.svg" alt="Pour archiver" />
              <button on:click={() => deleteList(todo)}>
                <img class="list-button"
                     src="/src/assets/delete.svg"
                     alt="Pour supprimer">
              </button>
            </div>
          </li>
        {/if}
      {/each}
    </ul>
  </section>
{/if}
