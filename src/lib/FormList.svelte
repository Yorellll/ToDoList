<script lang="ts" context="module">
  import { navigate } from "svelte-routing";

  // Type Liste pour la création de la liste
  export type typeListe = {
    title: String;
    urlTitle: String;
    archive: Boolean;
    todos: taskType[];
  };

   export type taskType ={
    task: string,
    check: boolean
  }

  // Définition des variables initiales
  let title = "";

  let todos: typeListe[] = JSON.parse(
    localStorage.getItem("todosList") || "[]"
  );

  const pattern = /[^a-zA-Z0-9\u00C0-\u017F]+/g;

  // Création d'une liste et récupération de son nom
  const redirectAndCreate = () => {
    // Définition de la liste
    const list = {
      title: title,
      urlTitle: title.replace(pattern, "-"),
      archive: false,
      todos: [],
    };

    // Ajout de la liste dans notre todos qui est vide ou qui récupère les listes déjà existantes
    // Et on rajoute à todos la liste créée
    todos = [...todos, list];
    localStorage.setItem("todosList", JSON.stringify(todos));

    // Redirection vers la liste créée
    // .tolowerCase() pour éviter les problèmes de casse
    navigate(`/${title.replace(pattern, "-")}`);
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
<!-- Pas besoin de générer un page.ts et +page.svelteHTML
Mettre son dossier dans un dossier [todo]
faire le fichier +page.svelte
Récupérer le params de title
-->
