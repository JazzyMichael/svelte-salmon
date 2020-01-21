<script>
</script>

<form class="post-form">
    <input type="text" placeholder="Title">

    <textarea name="description" rows="3" placeholder="Description"></textarea>

    <button
        on:click={() => console.log('posted')}>
        Submit
    </button>
</form>

<style>
.post-form {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    max-width: 666px;
    margin: auto;
}

.post-form > * {
    width: 100%;
}
</style>
