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

    #top {
        display: flex;
        align-items: center;
        justify-content: center;
        background-color: #F8D553;
        height: 4em;   
        width: 100%;
    }

    select {
        padding: 5px;
        margin: 5px 5px;
        border-radius: 5px;
    }

    #results {
        display: flex;
        background-color:#054A91;
        justify-content: center;
        column-gap: 10px;
        height: 100%;
        width: 100%;
    }

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

<div id='main-wrapper'>
    <h1>Pokemon Gen 2-4 Type Effectiveness Lookup PTEL</h1>
    <div id='top'>
        <select name='gen'>
            <option value='4'>4</option>
        </select>
        <select name='type1'>
            <option value='fire'>Fire</option>
            <option value='water'>Water</option>
            <option value='grass'>Grass</option>
        </select>
        <select name='type2     '>
            <option value='fire'>Fire</option>
            <option value='water'>Water</option>
            <option value='grass'>Grass</option>
        </select>
    </div>
    <div id='results'>
        <div class='2Resist card'>
            <p>2X Resistance</p>
            <div></div>
        </div>
        <div class='notEffective card'>
            <p>Not Effected By</p>
            <div></div>
        </div>
        <div class='2Effective card'>
            <p>2X Effectiveness</p>
            <div></div>
        </div>
    </div>
</div>