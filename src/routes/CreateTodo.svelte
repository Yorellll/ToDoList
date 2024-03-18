<script lang="ts">
  import type { typeListe } from "../lib/FormListe.svelte";

  // Type pour la lsite à afficher
  let todos: typeListe[];

  // Définition des variables initiales
  let todoTitle = "";

  // Récupération de l'url en la formattant pour récupérer le nom de la liste
  const location = document.location.pathname.split("/")[1];

  // Pour récupérer les todos
  todos = JSON.parse(localStorage.getItem("todosList") || "[]");
  // console.log(todos);

  // Stock et récupère la liste à afficher qui correspond à mon url
  let todoToShow = todos.find((todo) => todo.urlTitle === location);

  console.log(todoToShow);

  const addTodo = () => {
    // Ajout de la nouvelle tâche dans la liste todos
    todoToShow?.todos.push(todoTitle);
    // Mise à jour de la liste dans le localStorage
    localStorage.setItem("todosList", JSON.stringify(todos));
  };
</script>

<div class=" create container">
  {#if todoToShow}
    <h1 class="big-title">{todoToShow.title}</h1>
    <div class="create-input">
      <label for="title">Nom de la liste</label>
      <input
        id="title"
        type="text"
        bind:value={todoTitle}
        placeholder="Repas, Achat Vélo, Gateau au chocolat..."
      />
      <button class="btn btn-header" on:click={addTodo}>Créer</button>
    </div>
  {/if}
</div>
