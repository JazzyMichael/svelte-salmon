<script>
    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();

    export let userId;

    let text = '';

    const submit = () => {
        dispatch('submit', {
            text,
            userId,
            createdAt: Date.now()
        });
        resetInputs();
    }

    const resetInputs = () => {
        text = '';
    }
</script>

<input bind:value={text} type="text" placeholder="Title">

<button on:click={submit} disabled={!text || !userId}>📜 Add Comment</button>

<style>
* {
    width: 100%;
    max-width: 666px;
    display: block;
    margin: 1em auto;
}
</style>
