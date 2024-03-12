<script lang='ts'>
    import TypeImage from "./TypeImage.svelte";
    export let eff;
    export let type;
</script>

<style lang='scss'>
    .card{
        display: flex;
        flex-direction: column;
        align-items: center;
        margin: 8px;
        padding: 8px;
        width:140px;
        height: 70px;
        border: 2px solid black;
        background-color: $white;
        border-radius: 5px;
    }

    p {
        margin: 5px;
        font-size: 1.1rem;
    }

    .Normal {background-color: $Normal;}
    .Fighting {background-color: $Fighting;}
    .Flying {background-color: $Flying;}
    .Ground {background-color: $Ground;}
    .Poison {background-color: $Poison;}
    .Rock {background-color: $Rock;}
    .Bug { background-color: $Bug;}
    .Ghost { background-color: $Ghost;}
    .Steel { background-color: $Steel;}
    .Fire { background-color: $Fire;}
    .Water { background-color: $Water;}
    .Grass { background-color: $Grass;}
    .Electric { background-color: $Electric;}
    .Psychic { background-color: $Psychic;}
    .Ice { background-color: $Ice;}
    .Dragon { background-color: $Dragon;}
    .Dark { background-color: $Dark;}
    .Fairy { background-color: $Fairy;}
</style>

<div class='card {type}'>
    <TypeImage src='../{type}.png' />
    {#if eff !== 0}
        <p>{eff}X Effective</p>
    {:else}
        <p>No Effect</p>
    {/if}
</div>