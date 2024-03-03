<script>
    import { SvelteComponentTyped, createEventDispatcher } from "svelte";
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
        font-size:large;
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
<select bind:value={selected} on:change={genUpdate} id='genSelect'>
    {#each genVals as gen}
        <option value={gen}>{gen}</option>
    {/each}>
    </select>