<script>
    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();

    export let userId;

    let title = '';
    let description = '';

    const submit = () => {
        dispatch('submit', {
            title,
            description,
            userId,
            createdAt: Date.now()
        });
        resetInputs();
    }

    const resetInputs = () => {
        title = '';
        description = '';
    }
</script>

<input bind:value={title} type="text" placeholder="Title">

<textarea bind:value={description} rows="3" placeholder="Description"></textarea>

<button on:click={submit} disabled={!title || !description || !userId}>Submit</button>

<style>
* {
    width: 100%;
    max-width: 666px;
    display: block;
    margin: 1em auto;
}
</style>
