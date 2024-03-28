<script lang="ts">
<<<<<<< HEAD
  import type { typeListe } from "../lib/FormList.svelte";
=======
    import type { taskType, typeListe} from "../lib/FormListe.svelte";
>>>>>>> 38426f6 (Ajout de la fonction achieved)

  // Type pour la lsite à afficher
  let todos: typeListe[];

<<<<<<< HEAD
  // Définition des variables initiales
  let todoTitle = "";
=======
    //Task courante que l'on ajoute, du type taskType ajouté dans FormListe
    const todoTitle: taskType = {
        task:"",
        check: false
    }
>>>>>>> 38426f6 (Ajout de la fonction achieved)

  // Récupération de l'url en la formattant pour récupérer le nom de la liste
  const location = document.location.pathname.split("/")[1];

  // Pour récupérer les todos
  todos = JSON.parse(localStorage.getItem("todosList") || "[]");
  // console.log(todos);

  // Stock et récupère la liste à afficher qui correspond à mon url
  let todoToShow = todos.find((todo) => todo.urlTitle === location);

<<<<<<< HEAD
  console.log(todoToShow);

  const addTodo = () => {
    if (todoTitle) {
      // Ajout de la nouvelle tâche dans la liste todos
      todoToShow?.todos.push(todoTitle);
      // Mise à jour de la liste dans le localStorage
      localStorage.setItem("todosList", JSON.stringify(todos));
      todos = JSON.parse(localStorage.getItem("todosList") || "[]");
      todoToShow = todos.find((todo) => todo.urlTitle === location);
      todoTitle = "";
    }
  };
=======

    console.log(todoToShow);

    const addTodo = () => {
        if (todoTitle.task) {
            // Ajout de la nouvelle tâche dans la liste todos
            todoToShow?.todos.push(todoTitle);
            // Mise à jour de la liste dans le localStorage
            localStorage.setItem("todosList", JSON.stringify(todos));
            todos = JSON.parse(localStorage.getItem("todosList") || "[]");
            todoToShow = todos.find((todo) => todo.urlTitle === location);
            todoTitle.task = "";
        }
    };


    //Cette fonction est appelé à l'update du check, elle met a jour son état et la met à jour dans le stockage
    const changeCheckState = (  nameTask: string,state:boolean) => {
        if (todoToShow){
            todoToShow.todos.forEach(function (task){
                if (task.task == nameTask){
                    task.check = state;
                    localStorage.setItem("todosList", JSON.stringify(todos));
                    todos = JSON.parse(localStorage.getItem("todosList") || "[]");
                    todoToShow = todos.find((todo) => todo.urlTitle === location);
                }

            })
        }
    }
>>>>>>> 38426f6 (Ajout de la fonction achieved)

  let check = true;
  let checkboxStyle = "";

  //class css activer en fonction du check ou non de la checkbox +ajouter un bool dans la liste
  //de task afin d'enregistrer en session storage l'état de chaque checkbox
</script>

<div class=" create container">
<<<<<<< HEAD
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

{#if todoToShow && todoToShow.todos}
  <div class="  task container">
    {#each todoToShow.todos as task}
      <li class="list">
        <label class="nameTask" for="did">{task}</label>
        <input id="check" type="checkbox" name="did" bind:checked={check} />
        <!--{#if (check)}-->
        <!--    {@const checkboxStyle = "text-decoration: line-through;"}-->
        <!--{:else}-->
        <!--    {@const checkboxStyle = ""}-->
        <!--{/if}-->
      </li>
    {/each}
  </div>
{/if}
=======
    {#if todoToShow}
        <h1 class="big-title">{todoToShow.title}</h1>
        <div class="create-input">
            <label for="title">Nom de la liste</label>
            <input
                    id="title"
                    type="text"
                    bind:value={todoTitle.task}
                    placeholder="Repas, Achat Vélo, Gateau au chocolat..."
            />
            <button class="btn btn-header" on:click={addTodo}>Créer</button>

        </div>
    {/if}
</div>

{#if (todoToShow && todoToShow.todos)}
    <div class="  task container">
        {#each todoToShow.todos as taskCourante}
            <li class="list">
                <label class="nameTask" for="did" class:achievedTask={taskCourante.check}>{taskCourante.task}</label>
                <input
                       id="check" type="checkbox"
                       name="did"
                       bind:checked={taskCourante.check}
                       on:change={() => changeCheckState(taskCourante.task, taskCourante.check)}
                       >
            </li>
        {/each}
    </div>
{/if}
>>>>>>> 38426f6 (Ajout de la fonction achieved)
