<script>
    import GenSelect from "./GenSelect.svelte";
    import TypeSelect from "./TypeSelect.svelte";
    let t1 = 'type1';
    let t2 = 'type2';
</script>

<style>
    #selector {
        display: flex;
        align-items: center;
        justify-content: center;
        background-color: #F8D553;
        height: 4em;   
        width: 100%;
    }
</style>

<div id='selector'>
    <GenSelect></GenSelect>
    {#each [t1, t2] as id_}
        <TypeSelect id={id_} on:typeUpdate/>
    {/each}
</div>