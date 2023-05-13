<script>
    import { createEventDispatcher } from "svelte";
    import { v4 } from "uuid";

    const dispatcher = createEventDispatcher();

    let name = "";
    let description = "";

    const createTask = () => {
        if (!name && !description) {
            alert("please enter either name or description!");
            return;
        }

        dispatcher("createTask", {
            id: v4(),
            name,
            description,
        });

        name = "";
        description = "";
    };
</script>

<div style="display: flex; justify-content: center;">
    <div class="task-card">
        <input
            type="text"
            bind:value={name}
            style="border: none; background: none; padding: 0.5rem; outline: none; width: 100%;"
            placeholder="Enter Task name"
        />

        <hr style=" width: 100%; margin: 1rem 0;" />

        <textarea
            bind:value={description}
            style=" height: 10rem; border: none; background: none; resize: none;  padding: 0.5rem; outline: none;  width: 100%;"
            placeholder="Describe Your Task"
        />

        <button style="width: 30%;" on:click={createTask}>Create</button>
    </div>
</div>

<style>
    .task-card {
        display: flex;
        align-items: flex-end;
        width: 70%;
        background-color: white;
        padding: 2rem;
        flex-direction: column;
        margin-bottom: 2rem;
        border-radius: 0.5rem;
        border: 0.1rem solid grey;
        box-shadow: 0.1rem 0.1rem 0.5rem grey;
    }

    button {
        padding: 0.6rem;
        cursor: pointer;
    }

    @media(max-width: 76rem){
        .task-card {
            width: 90%;
        }
    }
</style>
