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
        font-size: 1.2em;
        border: 5px;
        margin: 5px;
        padding: 5px;
        border-radius: 5px;
    }
</style>

<label for={id}>
    {#if id=='type1'}
    Type 1: {:else} Type 2
    {/if}
</label>
<select bind:value={selected} on:change={typeUpdate} id={id}>
    {#each types as type}
        <option>{type}</option>
    {/each}
</select>