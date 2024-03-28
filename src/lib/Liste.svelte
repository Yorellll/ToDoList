<script lang="ts">
  import App from "../App.svelte";
  import type { typeListe } from "./FormList.svelte";
  // Le type des todos à été difini à la création, je le récupère juste
  let listeTodos: typeListe[];
  let listTask: String[];

  const getTodos = () => {
    // Pour récupérer les todos
    listeTodos = JSON.parse(localStorage.getItem("todosList") || "[]");
  };

  const archiveTodo = (todo: typeListe) => {
    const archive = todo.archive;

    todo.archive = !archive;

    localStorage.setItem("todosList", JSON.stringify(listeTodos));

    getTodos();
  };

  // Faire un bouton qui cache les todos archivés
  // Avec du style et ajouter ou supprimer la classe

  // Déclenchenement de la fonction getTodos
  getTodos();
</script>

<!-- <button on:click={getTodos}>Refresh</button> -->
<section class="lists container">
  <h2 class="big-title">Listes actives</h2>
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
  </ul>
</section>

<section class="lists archives container">
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
