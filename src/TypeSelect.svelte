<script>
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();
    export let id;
    let selected;
    export let types = ['Fire', 'Water', 'Grass'];

    function typeUpdate(event) {
        dispatch('typeUpdate', {
            'selected':selected,
            'id':event.target.id,
        });
        // console.log(event.target.id);
    }
</script>

<!-- {@debug selected} -->

<style>
    select {
    padding: 5px;
    margin: 5px 5px;
    border-radius: 5px;
    }
</style>

<select bind:value={selected} on:change={typeUpdate} id={id}>
    {#each types as type}
        <option>{type}</option>
    {/each}
</select>