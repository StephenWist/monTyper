<script lang='ts'>
    import AutoComplete from "simple-svelte-autocomplete";
    import { createEventDispatcher } from "svelte";
    import { gen } from "./stores";
    const dispatch = createEventDispatcher();
    let genVals=['1','2-5','6+'];
    let selected;
    
    function genUpdate(event) {
        dispatch('genUpdate', {
            'selected':selected,
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
<div>
    <label for='genSelect'>Generation:</label>
    <AutoComplete onChange={genUpdate} items="{genVals}" bind:value={$gen} bind:selectedIem="{selected}" id='genSelect' />
</div>