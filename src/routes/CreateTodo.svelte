<script lang="ts">
  import BreadCrumb from "../lib/BreadCrumb.svelte";
  import { pattern, withoutAccent, type taskType, type typeListe } from "../lib/FormList.svelte";

  // Type pour la lsite à afficher
  let todos: typeListe[];

  //Task courante que l'on ajoute, du type taskType ajouté dans FormListe
  const todoTitle: taskType = {
    task: "",
    check: false,
    date: Date.now(),
  };

  let tab: any = [];

  // Récupération de l'url en la formattant pour récupérer le nom de la liste
  const location = document.location.pathname.split("/")[1];
  const subLocation = document.location.pathname.split("/")[2];

  // Pour récupérer les todos
  todos = JSON.parse(localStorage.getItem("todosList") || "[]");

  // Stock et récupère la liste à afficher qui correspond à mon url
  $: todoToShow = todos.find((todo) => todo.urlTitle === location) as typeListe;
  // console.log(todoToShow);

  const sortTodos = () => {
    tab = [...todoToShow.todos, ...todoToShow.subLists];

    tab.sort((a: typeListe, b: taskType) => b.date - a.date);

    return true;
  };

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
      todoTitle.date = Date.now();
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

  // Pour supprimer un élément de la list (pour la liste et la subListe)
  const deleteTodo = (nameTask: string, lists: typeListe) => {
    const removeElt = lists.todos.filter((task) => task.task !== nameTask);
    lists.todos = removeElt;
    localStorage.setItem("todosList", JSON.stringify(todos));
    todos = JSON.parse(localStorage.getItem("todosList") || "[]");
  };
  
  const deleteSubList = (nameTask: string, lists: typeListe[]) => {
    const removeSubList = lists.filter((obj) => obj.title !== nameTask);
    lists = removeSubList;  
    
    localStorage.setItem("todosList", JSON.stringify(todos));
    todos = JSON.parse(localStorage.getItem("todosList") || "[]");
  };
</script>

<div class="create container">
  {#if todoToShow && subLocation}
    {#each todoToShow.subLists as lists}
      {#if withoutAccent(lists.title.replace(pattern, "-").toLowerCase()) === subLocation}
        <BreadCrumb previousList={todoToShow} currentList={lists} />
        <h1 class="big-title">{lists.title}</h1>
        <div class="input-content">
          <div class="create-input">
            <label for="subTitleList">Nom de la sous-tâche</label>
            <input
              id="subTitleList"
              type="text"
              bind:value={todoTitle.task}
              placeholder="Oeufs, Farine, Sucre..."
              autocomplete="off"
            />
          </div>
          <button class="btn btn-header" on:click={() => addTodo(lists, false)}>Créer</button>
          {#if !subLocation}
            <button class="btn btn-header" on:click={() => addTodo(lists, true)}> Créer une liste secondaire</button>
          {/if}
        </div>
        {#each lists.todos as taskCourante}
          <div class="task">
            <div class="task-content">
              <label class="nameTask" for={taskCourante.task} class:achievedTask={taskCourante.check}>
                {taskCourante.task}
              </label>
              <input
                id={taskCourante.task}
                type="checkbox"
                name="did"
                bind:checked={taskCourante.check}
                on:change={() => changeCheckState(taskCourante.task, taskCourante.check)}
              />
            </div>
            <button on:click={() => deleteTodo(taskCourante.task, lists)}>
              <img src="/src/assets/x_icon.svg" alt="Supprimer la tâche" />
            </button>
          </div>
        {/each}
      {/if}
    {/each}
  {/if}
  {#if todoToShow && !subLocation}
    <h1 class="big-title">{todoToShow.title}</h1>
    <div class="input-content">
      <div class="create-input">
        <label for="titleList">Nom de la tâche</label>
        <input
          id="titleList"
          type="text"
          bind:value={todoTitle.task}
          placeholder="Repas, Achat Vélo, Gateau au chocolat..."
          autocomplete="off"
        />
      </div>
      <button class="btn btn-header" on:click={() => addTodo(todoToShow, false)}> Créer </button>
      {#if !subLocation}
        <button class="btn btn-special" on:click={() => addTodo(todoToShow, true)}> Créer une liste secondaire </button>
      {/if}
    </div>
  {/if}
</div>

{#if todoToShow && todoToShow.todos.length > 0 && !subLocation && sortTodos()}
  <div class="container">
    {#each tab as taskCourante}
      {#if taskCourante.task}
        <li class:achieve={taskCourante.check} class="task">
          <div class="task-content">
            <label class="nameTask" for={taskCourante.task} class:achievedTask={taskCourante.check}>
              {taskCourante.task}
            </label>
            <input
              id={taskCourante.task}
              type="checkbox"
              name="did"
              bind:checked={taskCourante.check}
              on:change={() => changeCheckState(taskCourante.task, taskCourante.check)}
            />
          </div>
          <button on:click={() => deleteTodo(taskCourante.task, todoToShow)}>
            <img src="/src/assets/x_icon.svg" alt="Supprimer la tâche" />
          </button>
        </li>
      {:else}
        <li class:achieve={taskCourante.check} class="task task-sub">
          <a
            class="nameTask"
            class:achievedTask={taskCourante.check}
            aria-label={`Lien vers ${taskCourante.title}`}
            href="/{taskCourante.urlTitle}"
          >
          <img src="/src/assets/chevron_icon.svg" alt="Lien vers la sous-liste">
            {taskCourante.title}
          </a>

          <button on:click={() => deleteSubList(taskCourante.title, tab)}>
            <img src="/src/assets/x_icon.svg" alt="Supprimer la tâche" />
          </button>
        </li>
      {/if}
    {/each}
  </div>
{/if}
