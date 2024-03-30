<script lang="ts">
  import {
    pattern,
    withoutAccent,
    type taskType,
    type typeListe
  } from "../lib/FormList.svelte";

  // Type pour la lsite à afficher
  let todos: typeListe[];

  //Task courante que l'on ajoute, du type taskType ajouté dans FormListe
  const todoTitle: taskType = {
    task: "",
    check: false,
    date: Date.now(),
  };

  // Récupération de l'url en la formattant pour récupérer le nom de la liste
  const location = document.location.pathname.split("/")[1];
  const subLocation = document.location.pathname.split("/")[2];

  // Pour récupérer les todos
  todos = JSON.parse(localStorage.getItem("todosList") || "[]");
  console.log(todos);

  // Stock et récupère la liste à afficher qui correspond à mon url
  $: todoToShow = todos.find((todo) => todo.urlTitle === location) as typeListe;
  // console.log(todoToShow);

  const addTodo = (currentTodo: typeListe, subList: boolean) => {
    if (todoTitle.task) {
      if (subList) {
        // Crée une sous liste
        let subList: typeListe = {
          title: todoTitle.task,
          urlTitle: currentTodo.urlTitle + "/" + withoutAccent(todoTitle.task.replace(pattern, "-").toLowerCase()),
          archive: false,
          todos: [],
          subLists: [],
          date: Date.now(),
        };
        // Insère la sous-liste dans la liste en cours
        currentTodo.subLists.push(subList);
        // console.log(currentTodo);
      } else {
        // Ajout de la nouvelle tâche dans la liste todos
        currentTodo.todos.push(todoTitle);
      }
      // Mise à jour de la liste dans le localStorage
      localStorage.setItem("todosList", JSON.stringify(todos));
      todos = JSON.parse(localStorage.getItem("todosList") || "[]");
      if (subLocation) {
        todoToShow.subLists.forEach((sub: typeListe) => {
          if (withoutAccent(sub.title.replace(pattern, "-").toLowerCase()) === subLocation) {
            todoToShow = sub;
          }
        });
      }
      todoTitle.task = "";
    }
  };

  //Cette fonction est appelé à l'update du check, elle met a jour son état et la met à jour dans le stockage
  const changeCheckState = (nameTask: string, state: boolean) => {
    if (todoToShow) {
      todoToShow.todos.forEach(function (task: taskType) {
        if (task.task == nameTask) {
          task.check = state;
          localStorage.setItem("todosList", JSON.stringify(todos));
          todos = JSON.parse(localStorage.getItem("todosList") || "[]");
          if (subLocation) {
            todoToShow.subLists.forEach((sub: typeListe) => {
              if (withoutAccent(sub.title.replace(pattern, "-").toLowerCase()) === subLocation) {
                todoToShow = sub;
              }
            });
          }
        }
      });
    }
  };
</script>

<div class=" create container">
  {#if todoToShow && subLocation}
    {#each todoToShow.subLists as lists}
      {#if withoutAccent(lists.title.replace(pattern, "-").toLowerCase()) === subLocation}
        <h1 class="big-title">{lists.title}</h1>
        <div class="input-content">
          <div class="create-input">
            <label for="title">Nom de la liste</label>
            <input
              id="title"
              type="text"
              bind:value={todoTitle.task}
              placeholder="Repas, Achat Vélo, Gateau au chocolat..."
            />
          </div>
          <button class="btn btn-header" on:click={() => addTodo(lists, false)}> Créer </button>
          {#if !subLocation}
            <button class="btn btn-header" on:click={() => addTodo(lists, true)}> Créer une liste secondaire </button>
          {/if}
        </div>
        <div class="container">
          {#each lists.todos as taskCourante}
            <div class="task">
              <label class="nameTask" for="did" class:achievedTask={taskCourante.check}>{taskCourante.task}</label>
              <input
                id="check"
                type="checkbox"
                name="did"
                bind:checked={taskCourante.check}
                on:change={() => changeCheckState(taskCourante.task, taskCourante.check)}
              />
            </div>
          {/each}
        </div>
      {/if}
    {/each}
  {/if}
  {#if todoToShow && !subLocation}
    <h1 class="big-title">{todoToShow.title}</h1>
    <div class="input-content">
    <div class="create-input">
      <label for="title">Nom de la liste</label>
      <input
        id="title"
        type="text"
        bind:value={todoTitle.task}
        placeholder="Repas, Achat Vélo, Gateau au chocolat..."
      />
    </div>
      <button class="btn btn-header" on:click={() => addTodo(todoToShow, false)}>Créer</button>
      {#if !subLocation}
        <button class="btn btn-special" on:click={() => addTodo(todoToShow, true)}>Créer une liste secondaire</button>
      {/if}
  </div>
  {/if}
</div>

{#if todoToShow && todoToShow.todos.length > 0 && !subLocation}
  {#if todoToShow.subLists}
    <div class="subList container">
      <h2>Vos listes secondaires</h2>
      {#each todoToShow.subLists as currentList}
        <div class="list">
          <a aria-label={`Lien vers ${currentList.title}`} href="/{currentList.urlTitle}">
            <h3>{currentList.title}</h3>
          </a>
        </div>
      {/each}
    </div>
  {/if}
  <div class="container">
    {#each todoToShow.todos as taskCourante}
      <li class:achieve={taskCourante.check} class="task">
        <label class="nameTask" for="did" class:achievedTask={taskCourante.check}>{taskCourante.task}</label>
        <input
          id="check"
          type="checkbox"
          name="did"
          bind:checked={taskCourante.check}
          on:change={() => changeCheckState(taskCourante.task, taskCourante.check)}
        />
      </li>
    {/each}
  </div>
{/if}
