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
  let liste: Liste[] = [];

  //   Création d'une liste et récupération de son nom
  const redirectAndCreate = () => {
    liste.push({ title: title, archive: false, todos: [] });
    localStorage.setItem("todos", JSON.stringify(liste));

    // Redirection vers la liste créée
    navigate(`/${title}`);
  };
</script>

<input id="Title" type="text" bind:value={title} />
<button on:click={redirectAndCreate}>Créer</button>
