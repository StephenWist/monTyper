<script lang='ts'>
    import GenSelect from "./GenSelect.svelte";
    import TypeSelect from "./TypeSelect.svelte";
    export let t1 = 'type1';
    export let t2 = 'type2';

</script>

<style lang='scss'>
    #selector {
        display: flex;
        align-items: center;
        justify-content: center;
        background-color: rgb(135,135,135);
        flex-wrap: wrap;
        border: 0.5px solid black;
        border-radius: 2px;
        box-shadow: 0.5px 0.5px 0.5px black;
        row-gap: 10px;
        padding: 8px;
    }
</style>

<div id='selector'>
    <GenSelect on:genUpdate />
    {#each [t1, t2] as id_}
        <TypeSelect bind:id={id_} on:typeUpdate/>
    {/each}
</div>