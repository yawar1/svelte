<script lang="ts">
    import { onMount } from "svelte";
    import CreateTask from "./CreateTask.svelte";
    import Task from "./Task.svelte";

    let tasks = [];

    const saveLocally = () => {
        localStorage.setItem("tasks", JSON.stringify(tasks));
    };

    const createTask = (task) => {
        tasks = [task, ...tasks];
        saveLocally();
    };

    const deleteTask = (taskId: string) => {
        console.log("taskId: ", taskId);
        tasks = tasks.filter((task) => task.id !== taskId);
        saveLocally();
    };

    const changeTaskStatus = (taskId: string, status) => {
        console.log("taskId: ", taskId);
        tasks = tasks.map((task) => {
            if (task.id === taskId) {
                return {
                    ...task,
                    status,
                };
            }

            return task;
        });

        saveLocally();
    };

    // $: {
    //     localStorage.setItem("tasks", JSON.stringify(tasks));
    // }

    onMount(() => {
        console.log("on mount called...");

        const tasksLocal = localStorage.getItem("tasks");
        if (tasksLocal) {
            tasks = JSON.parse(tasksLocal);
        }
    });
</script>

<div>
    <h1 style="padding: 2rem">Create Task</h1>

    <CreateTask on:createTask={({ detail }) => createTask(detail)} />

    <h1 style="padding: 2rem">All Tasks</h1>

    <hr />

    {#if tasks.length === 0}
        <h1 style="padding: 2rem; text-align: center;"> 🥺 No Tasks Found.</h1>
    {/if}

    <div class="task-viewer">
        {#each tasks as task (task.id)}
            <Task
                on:delete-task={({ detail }) => deleteTask(detail)}
                on:change-task-status={({ detail }) =>
                    changeTaskStatus(detail.id, detail.status)}
                {task}
            />
        {/each}
    </div>
</div>

<style>
    .task-viewer {
        padding: 2rem;
        display: grid;
        grid-template-columns: 32% 32% 32%;
        gap: 0.1rem 2%;
    }

    @media (max-width: 76rem) {
        .task-viewer {
            grid-template-columns: 100%;
            gap: 0;
        }
    }
</style>
