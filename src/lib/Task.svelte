<script>
    import { createEventDispatcher } from "svelte";

    export let task;

    const dispatch = createEventDispatcher();
</script>

<div class="task-card">
    <div style="display: flex;  margin-bottom: 1rem;">
        {#if task.status === "completed"}
            <p><del>{task.name}</del></p>
        {:else}
            <p>{task.name}</p>
        {/if}
    </div>

    <div
        style=" margin-bottom: 1rem; display: flex; width: 100%; height: 5rem; flex: 1;"
    >
        {#if task.status === "completed"}
            <p><del>{task.description}</del></p>
        {:else}
            <p>{task.description}</p>
        {/if}
    </div>

    <div class="task-card-header">
        <div style="display: flex; justify-content: space-between;">
            <button
                style="background: none;cursor: pointer;"
                on:click={() => dispatch("delete-task", task.id)}
            >
                🪣 &nbsp; Delete
            </button>
        </div>

        {#if task.status === "completed"}
            <div>
                <button
                    on:click={() =>
                        dispatch("change-task-status", {
                            id: task.id,
                            status: "pending",
                        })}>Un Done</button
                >
            </div>
        {:else}
            <div>
                <button
                    on:click={() =>
                        dispatch("change-task-status", {
                            id: task.id,
                            status: "completed",
                        })}>Done</button
                >
            </div>
        {/if}
    </div>
</div>

<style>
    .task-card {
        display: flex;
        /* width: 30%; */
        background-color: white;
        padding: 1rem;
        flex-direction: column;
        margin-bottom: 2rem;
        border-radius: 0.5rem;
        border: 0.1rem solid grey;
        box-shadow: 0.1rem 0.1rem 0.5rem grey;
    }

    .task-card-header {
        width: 100%;
        display: flex;
        justify-content: space-between;
    }

    p {
        font-size: 1.6rem;
    }
</style>
