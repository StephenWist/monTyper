<style>
    /* colors
        red: FF5353
        white: ffffff
        mustard: F8D553
        blue: 054A91
        black: 332E3C
    */

    #main-wrapper {
        display: flex;
        flex-direction: column;
        align-items: center;
        font-size: 14px;
        row-gap: 10px;
        margin: 10px;
        font-family: Helvetica, "Trebuchet MS", Verdana, sans-serif;
        background-color: #ffffff;
        height: 100vh;
        width: 100vw;
    }

    #results {
        display: flex;
        background-color:#054A91;
        justify-content: center;
        column-gap: 10px;
        height: 100%;
        width: 100%;
    }

</style>

<script>
    import Card from "../Card.svelte";
    import Selector from "../Selector.svelte";
    
    let super_effective = {
        'Fire': {'Grass': 2},
        'Water': {'Fire': 2},
        'Grass': {'Water': 2}
    };

    function typeUpdated(event) {
        alert(event.detail);
    };
</script>

<div id='main-wrapper'>
    <h1>Pokemon Gen 2-4 Type Effectiveness Lookup PTEL</h1>
    <Selector on:typeUpdate={typeUpdated} />

    <div id='results'>
        <Card></Card>
    </div>
</div>