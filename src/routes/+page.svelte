<script lang="ts">

    import FormTask from './app/FormTaskList.svelte'
    import type {Filter, Task} from "./app/Export/types.ts"
    import TaskList from "./app/Task-List.svelte"

    let currentFilter = $state<Filter>("all")
    let tasks = $state<Task[]>([])
    let totalDone = $derived(tasks.reduce(
        (total, task) => total + Number(task.done),0
    ))

    let filteredTasks = $derived.by(() => {
        switch (currentFilter) {
            case "all": {
                return tasks
            }
            case "done": {
                return tasks.filter(task => task.done)
            }
            case "todo": {
                return tasks.filter(task => !task.done)
            }
        }

        return tasks
    })

    function removeTask(id: string) {
        const index = tasks.findIndex(task => task.id === id)
        tasks.splice(index, 1)
    }

    function addTask(newTask: string) {

        tasks.push({
            id: crypto.randomUUID(),
            title: newTask,
            done: false,
        })

    }

    function toggleDone(task: Task) {

        task.done = !task.done

    }

</script>

{#snippet filterButton(filter: Filter)}

    <button onclick={() => currentFilter = filter}
            class:constrast={currentFilter === filter}
            class="secondary filterButton">{filter}
    </button>

{/snippet}

<main>

    <h1>Task App</h1>

    <FormTask {addTask} />
    <p>
        {#if tasks.length}
            {totalDone} / {tasks.length} tasks completed
        {:else}
            Add a task to get started.
        {/if}
    </p>
    {#if tasks.length}
        <div class="button-container">
            {@render filterButton("all")}
            {@render filterButton("todo")}
            {@render filterButton("done")}
        </div>
    {/if}
    <TaskList tasks={filteredTasks} {toggleDone} {removeTask} />

</main>

<style>

    main{
        margin: 1rem auto;
        max-width: 700px;
    }

    .button-container{
        display: flex;
        justify-content: end;
        margin-bottom: 1rem;
        gap: .5rem;
    }

    .filter-button{
        text-transform: capitalize;
    }

</style>