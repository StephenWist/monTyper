<script>
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();
    export let id;
    export let selected;
    export let types = ['Normal','Fighting','Ghost'];

    function typeUpdate(event) {
        dispatch('typeUpdate', {
            'selected':selected,
            'id':event.target.id,
        });
    }
</script>

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