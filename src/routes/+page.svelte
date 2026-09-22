<script lang="ts">

    import FormTask from './app/FormTaskList.svelte'
    import type { Task } from "./app/Export/types.ts"
    import TaskList from "./app/Task-List.svelte"

    let tasks = $state<Task[]>([])
    let totalDone = $derived(tasks.reduce(
        (total, task) => total + Number(task.done),0
    ))

    function removeTask(index: number) {
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

<main>

    <h1>Task App</h1>

    <FormTask {addTask} />
    {#if tasks.length}
        <p> {totalDone} / {tasks.length} tasks completed</p>
    {:else}
        <p>Add a task to get started.</p>
    {/if}
    <TaskList {tasks} {toggleDone} {removeTask} />

</main>

<style>

    main{
        margin: 1rem auto;
        max-width: 700px;
    }

</style>