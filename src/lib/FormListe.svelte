<script lang="ts" context="module">
  import { navigate } from "svelte-routing";

  // Type Liste pour la création de la liste
  export type typeListe = {
    title: String;
    archive: Boolean;
    todos: String[];
  };

  // Définition des variables initiales
  let title = "";

  let todos: typeListe[] = JSON.parse(
    localStorage.getItem("todosList") || "[]"
  );

  // Création d'une liste et récupération de son nom
  const redirectAndCreate = () => {
    // Définition de la liste
    const list = {
      title: title,
      archive: false,
      todos: [],
    };

    // Ajout de la liste dans notre todos qui est vide ou qui récupère les listes déjà existantes
    // Et on rajoute à todos la liste créée
    todos = [...todos, list];
    localStorage.setItem("todosList", JSON.stringify(todos));

    // Redirection vers la liste créée
    // .tolowerCase() pour éviter les problèmes de casse
    navigate(`/${title.toLowerCase()}`);
  };
</script>

<section class="create container">
  <h1 class="big-title">Créer une liste</h1>
  <div class="create-input">
    <label for="title">Nom de la liste</label>
    <input
      id="title"
      type="text"
      bind:value={title}
      placeholder="Repas, Achat Vélo, Gateau au chocolat..."
    />
    <button class="btn btn-header" on:click={redirectAndCreate}>Créer</button>
  </div>
</section>
