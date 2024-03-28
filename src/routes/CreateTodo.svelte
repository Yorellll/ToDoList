<script lang="ts">
    import type { taskType, typeListe} from "../lib/FormListe.svelte";

    // Type pour la lsite à afficher
    let todos: typeListe[];

    //Task courante que l'on ajoute, du type taskType ajouté dans FormListe
    const todoTitle: taskType = {
        task:"",
        check: false
    }

    // Récupération de l'url en la formattant pour récupérer le nom de la liste
    const location = document.location.pathname.split("/")[1];

    // Pour récupérer les todos
    todos = JSON.parse(localStorage.getItem("todosList") || "[]");
    // console.log(todos);

    // Stock et récupère la liste à afficher qui correspond à mon url
    let todoToShow = todos.find((todo) => todo.urlTitle === location);


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


</script>

<div class=" create container">
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