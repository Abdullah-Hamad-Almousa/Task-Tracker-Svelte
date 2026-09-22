<script lang="ts">

    import {fade} from "svelte/transition"
    import type { Task } from "./Export/types.ts"

    let { tasks, toggleDone, removeTask }: {
        tasks: Task[]
        toggleDone: (task: Task) => void
        removeTask: (id: string) => void
    } = $props()

</script>

<section>

    {#each tasks as task (task)}

        <article class="task" transition:fade>

            <label>
                <input
                        type="checkbox"
                        checked={task.done}
                        onchange={() => toggleDone(task)}
                />
                <span class:done={task.done}>{task.title}</span>
            </label>

            <button class="outline" onclick={() => removeTask(task.id)}>Remove</button>

        </article>

    {/each}

</section>

<style>

    .task {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .done {
        text-decoration: line-through;
    }

</style>