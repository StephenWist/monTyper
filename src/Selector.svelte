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
        background-color: $mustard;
        flex-wrap: wrap;
        // width: 100%;
        // margin: 8px;
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