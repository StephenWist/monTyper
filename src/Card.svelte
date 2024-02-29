<script>
    let className = 'card';
    export { className as class };
    export let eff;
</script>

<style>
    .card{
        display: flex;
        flex-direction: column;
        align-items: center;
        margin: 10px;
        width:140px;
        height: 70px;
        background-color: rgb(226, 236, 225);
        border-radius: 5px;
    }

    .card>div {
        height:20px;
        width:70px;
        background-color: #332E3C;
        border-radius: 5px;
    }
</style>

<div class={className}>
    {#if eff == 'se'}
        <p>2X Effective</p>
    {:else if eff == 'ne'}
        <p>2X Resist</p>
    {:else}
        <p>No Effect</p>
        <p>{eff}</p>
    {/if}
    <div></div>
</div>