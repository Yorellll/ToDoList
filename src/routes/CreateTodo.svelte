<script lang="ts">
  import type { typeListe } from "../lib/FormListe.svelte";

  // Récupération de l'url en la formattant pour récupérer le nom de la liste
  const location = document.location.pathname.split("/")[1];
  // console.log(location);

  let todos: typeListe[];

  // Pour récupérer les todos
  todos = JSON.parse(localStorage.getItem("todosList") || "[]");

  // console.log(todos);

  // Stock et récupère la liste à afficher qui correspond à mon url
  let todoToShow = todos.find((todo) => todo.title.toLowerCase() === location);

  console.log(todoToShow);
</script>

<div class="container">
  {#if todoToShow}
    <h1>{todoToShow.title}</h1>
  {/if}
  <p>
    Lorem, ipsum dolor sit amet consectetur adipisicing elit. Cum sed, nihil
    quos sint numquam tenetur repudiandae veniam blanditiis magnam. Repellat
    accusamus aut soluta beatae. Saepe, aliquam hic. Ad aperiam, voluptate
    veniam officiis at praesentium! Facilis, magni est! Ducimus consequatur
    incidunt quasi pariatur dolorem fugit laboriosam ullam dolores beatae
    mollitia eaque rerum asperiores accusamus aliquam quis dolore, amet
    temporibus qui harum?
  </p>
</div>
