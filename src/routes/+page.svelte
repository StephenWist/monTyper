<style>
    /* colors
        red: FF5353
        white: ffffff
        mustard: F8D553
        blue: 054A91
        black: 332E3C
    */

    body {
        margin: 0;
    }

    #main-wrapper {
        display: flex;
        flex-direction: column;
        align-items: center;
        font-size: 14px;
        row-gap: 10px;
        font-family: Helvetica, "Trebuchet MS", Verdana, sans-serif;
        background-color: #ffffff;
        height: 100vh;
        width: 100vw;
    }

    #result-title {
        display: flex;
        gap: 20px;
        background-color: #ffffff;
    }

    #results {
        display: flex;
        flex-direction: row;
        background-color:#054A91;
        flex-wrap: wrap;
        justify-content: center;
        align-content: flex-start;
        column-gap: 8px;
        height: 100%;
        width: 60%;
    }
</style>

<script lang='ts'>
    import Card from "../Card.svelte";
    import Selector from "../Selector.svelte";
    import _ from 'lodash';
    
    let type_effectiveness = {
        'Normal': { 'Fighting': 2,
                        'Ghost': 0
                    
        },
        'Ghost': {
                    'Normal':0,
                    'Fighting':0,
                    'Poison':0.5,
                    'Bug':0.5,
                    'Ghost':2,
                    'Dark':2
                
        },
        'Fighting': {
                'Flying':2,
                'Rock':0.5,
                'Bug':0.5,
                'Psychic':2,
                'Dark':0.5
        },
    };

    // init types
    let t1 = 'Normal';
    let t2 = 'Normal';
    let dual_type = {};
    
    // alphabetic sort
    let comparer = function(a,b) {
            if (a[0] < b[0]) return -1;
            if (a[0] > b[0]) return 1;
            return 0;
        };

    function typeUpdated(event) {
        if (event.detail.id == 'type1') {
            t1 = event.detail.selected;
        } else {
            t2 = event.detail.selected;
        };

        if ( t1 == t2 ) {
            console.log('Same types, nothing to do.');
            return
        };

        dual_type = {};
        let t2e = {...type_effectiveness[t2]};
        let t1e = {...type_effectiveness[t1]};

        // get all types in common and exclusive
        let t1_keys = Object.keys(t1e);
        let t2_keys = Object.keys(t2e);
        let commonD = _.intersection(t1_keys, t2_keys);
        let uniqD = _.xor(t2_keys, t1_keys);

        // combine all unique types into final dual types object
        uniqD.forEach(t => { t1_keys.includes(t) ? dual_type[t] = t1e[t] : dual_type[t] = t2e[t]; });
        // calculate common type effectiveness
        commonD.forEach(t => { dual_type[t] =  t1e[t] * t2e[t]; });
        console.log('dual type ', dual_type);
    };
</script>

<div id='main-wrapper'>
    <h1>monTyper</h1>
    <h2>Look up a Pokemon's weaknesses</h2>
    <Selector on:typeUpdate={typeUpdated} />
    <div id='results'>
        {#if t1 != t2}
        {#await dual_type}
            <p>working...</p>
        {:then dt}
            <!-- Sort alphabetically -->
            {#each Object.entries(dt).sort(comparer) as [type, effectiveness]}
                <Card eff={effectiveness} type={type} />
            {/each}
        {/await}
        {/if}
    </div>
</div>