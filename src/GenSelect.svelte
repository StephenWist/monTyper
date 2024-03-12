<script lang='ts'>
    import { createEventDispatcher } from "svelte";
    import { gen } from "./stores";
    const dispatch = createEventDispatcher();
    export let genVals=['1','2-5','6+'];
    export let selected;
    
    function genUpdate(event) {
        dispatch('genUpdate', {
            'selected':selected,
            'id':event.target.id,
        });
    }
</script>

<style>
    label {
        font-size: 1.2em;
        border: 5px;
        margin: 5px;
        padding: 5px;
    }

    select {
    font-size: 1.2em;
    padding: 5px;
    border: 5px;
    margin: 5px 5px;
    border-radius: 5px;
    }
</style>

<label for='genSelect'>Generation:</label>
<select bind:value={$gen} on:change={genUpdate} id='genSelect'>
    {#each genVals as g}
        <option value={g}>{g}</option>
    {/each}>
    </select>