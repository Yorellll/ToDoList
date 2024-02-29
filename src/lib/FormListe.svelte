<script lang="ts">
  import { navigate } from "svelte-routing";

  // Type Liste pour la création de la liste
  type Liste = {
    title: String;
    archive: Boolean;
    todos: String[];
  };

  //   Définition des variables initiales
  let title = "";

  let todos: Liste[] = JSON.parse(localStorage.getItem("todosList") || "[]");

  //   Création d'une liste et récupération de son nom
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
    navigate(`/${title}`);
  };
</script>

<input id="Title" type="text" bind:value={title} />
<button on:click={redirectAndCreate}>Créer</button>
