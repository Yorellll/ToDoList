<script lang="ts">
  import type { typeListe } from "./FormListe.svelte";
  // Le type des todos à été difini à la création, je le récupère juste
  let listeTodos: typeListe[];

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
  <h1 class="big-title">Listes en activités</h1>
  <ul class="lists-content">
    {#if listeTodos}
      {#each listeTodos as todo}
        <li class="list">
          <h2>{todo?.title}</h2>
          <button>Supprimer</button>
          <button>Archiver</button>
        </li>
      {/each}
    {/if}
  </ul>
</section>
