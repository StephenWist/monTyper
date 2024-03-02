<script>
    import TypeImage from "./TypeImage.svelte";
    let className = 'card';
    export { className as class };
    export let eff;
    export let type;
</script>

<style>
    .card{
        display: flex;
        flex-direction: column;
        align-items: center;
        margin: 5px;
        width:140px;
        height: 70px;
        background-color: rgb(226, 236, 225);
        border-radius: 5px;
    }

    p {
        margin: 5px;
    }
</style>

<div class={className}>
    <TypeImage src='../{type}.png' />
    {#if eff != 0}
        <p>{eff}X Effective</p>
    {:else}
        <p>No Effect</p>
    {/if}
</div>