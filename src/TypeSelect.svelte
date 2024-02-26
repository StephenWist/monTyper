<script>
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();
    let id;
    let selected;
    export let types = ['Fire', 'Water', 'Grass'];

    function typeUpdate() {
        dispatch('typeUpdate', selected);
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

<select bind:value={selected} on:change={typeUpdate} name={id}>
    {#each types as type}
        <option>{type}</option>
    {/each}
</select>