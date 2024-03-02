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
  <h2 class="big-title">Listes actives</h2>
  <ul class="lists-content">
    {#if listeTodos}
      {#each listeTodos as todo}
        <li class="list">
          <a href="/{todo?.title}"><h3>{todo?.title}</h3></a>
          <button>Supprimer</button>
          <button>Archiver</button>
        </li>
      {/each}
    {/if}
  </ul>
</section>
