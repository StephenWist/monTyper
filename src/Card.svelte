<script>
    import TypeImage from "./TypeImage.svelte";
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
        border: 2px solid black;
        background-color: #ffffff;
        border-radius: 5px;
    }

    p {
        margin: 5px;
        font-size: 1.1rem;
    }

    .Normal {background-color: #929DA3;}
    .Fighting {background-color: #CE416B;}
    .Flying {background-color: #8FA9DE;}
    .Ground {background-color: #E19F87;}
    .Poison {background-color: #BF99D4;}
    .Rock {background-color: #d3c8aa;}
    .Bug { background-color: #add080;}
    .Ghost { background-color: #8e97c0;}
    .Steel { background-color: #90acb8;}
    .Fire { background-color: #ffb68d;}
    .Water { background-color: #8cade0;}
    .Grass { background-color: #94cb91;}
    .Electric { background-color: #f6dd83;}
    .Psychic { background-color: #fa9c9d;}
    .Ice { background-color: #9cdacd;}
    .Dragon { background-color: #7a99cf;}
    .Dark { background-color: #908d96;}
    .Fairy { background-color: #f2adec;}
</style>

<div class='card {type}'>
    <TypeImage src='../{type}.png' />
    {#if eff != 0}
        <p>{eff}X Effective</p>
    {:else}
        <p>No Effect</p>
    {/if}
</div>