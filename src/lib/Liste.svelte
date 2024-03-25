<script lang="ts">
  import type { typeListe } from "./FormList.svelte";
  // Le type des todos à été difini à la création, je le récupère juste
  let listeTodos: typeListe[];

  const getTodos = () => {
    // Pour récupérer les todos
    listeTodos = JSON.parse(localStorage.getItem("todosList") || "[]");
    // console.log(listeTodos);
  };

  const archiveTodo = (todo: typeListe) => {
    const archive = todo.archive;

    // On change la valeur de archive
    todo.archive = !archive;
    console.log(todo);

    localStorage.setItem("todosList", JSON.stringify(listeTodos));
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
      {/each}
    {/if}
  </ul>
</section>

<section class="archives container">
  <h3 class="big-title">Listes archivées</h3>
  <!-- {#if listeTodos}
    {#each listeTodos as todo}{/each}
  {/if} -->
  <ul class="lists-content"></ul>
</section>
