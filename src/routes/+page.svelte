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
        font-family: Helvetica, "Trebuchet MS", Verdana, sans-serif;
        background-color: #ffffff;
        height: 100vh;
        width: 100vw;
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
        width: 100%;
    }
</style>

<script lang='ts'>
    import { onMount } from 'svelte';
    
    import _ from 'lodash';
    
    import Card from "../Card.svelte";
    import Selector from "../Selector.svelte";

    // init types
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
        'Poison': {
            'Poison':0.5,
            'Ground':2,
            'Bug':0.5,
            'Grass':0.5,
            'Psychic':2
        },
        'Flying': {
            'Fighting':0.5,
            'Ground':0,
            'Rock':2,
            'Grass':0.5,
            'Electric':2,
            'Ice':2
        },
        'Ground': {
            'Poison':0.5,
            'Rock':0.5,
            'Water':2,
            'Grass':2,
            'Electric':0,
            'Ice':2
        },
        'Rock': {
            'Normal':0.5,
            'Fighting':2,
            'Flying':0.5,
            'Poison':0.5,
            'Ground':2,
            'Steel':2,
            'Fire':0.5,
            'Water':2,
            'Grass':2
        },
        'Bug': {
            'Fighting':0.5,
            'Flying':2,
            'Ground':0.5,
            'Rock':2,
            'Fire':2,
            'Grass':0.5
        },
        'Steel': {
            'Normal':0.5,
            'Fighting':2,
            'Flying':0.5,
            'Poison':0,
            'Ground':2,
            'Rock':0.5,
            'Bug':0.5,
            'Ghost':0.5,
            'Steel':0.5,
            'Fire':2,
            'Grass':0.5,
            'Psychic':0.5,
            'Ice':0.5,
            'Dragon':0.5,
            'Dark':0.5
        },
        'Fire': {
            'Ground':2,
            'Rock':2,
            'Bug':0.5,
            'Steel':0.5,
            'Fire':0.5,
            'Water':2,
            'Grass':0.5,
            'Ice':0.5
        },
        'Water': {
            'Steel': 0.5,
            'Fire': 0.5,
            'Water': 0.5,
            'Grass':2,
            'Electric':2,
            'Ice':0.5
        },
        'Grass': {
            'Flying':2,
            'Poison':2,
            'Ground':0.5,
            'Bug':2,
            'Fire':2,
            'Water':0.5,
            'Grass':0.5,
            'Electric':0.5,
            'Ice':2
        },
        'Electric': {
            'Flying': 0.5,
            'Ground':2,
            'Steel':0.5,
            'Electric':0.5
        },
        'Psychic': {
            'Fighting':0.5,
            'Bug':2,
            'Ghost':2,
            'Psychic':0.5,
            'Dark':2
        },
        'Ice': {
            'Fighting':2,
            'Rock':2,
            'Steel':2,
            'Fire':2,
            'Ice':0.5
        },
        'Dragon': {
            'Fire':0.5,
            'Water':0.5,
            'Grass':0.5,
            'Electric':0.5,
            'Ice':2,
            'Dragon':2
        },
        'Dark': {
            'Fighting':2,
            'Bug':2,
            'Ghost':0.5,
            'Psychic':0,
            'Dark':0.5
        }
    };

    let t1;
    let t2;
    let dual_type = {};

    // initial render after TypeSelectors are mounted
    onMount(() => {
        t2 = document.getElementById('type2').value;
        t1 = document.getElementById('type1').value;
        typeUpdated('onMount');
	});

    // alphabetic sort
    let comparer = function(a,b) {
            if (a[0] < b[0]) return -1;
            if (a[0] > b[0]) return 1;
            return 0;
        };

    function updateDualType() {
        // calculate dual type weaknesses

        let t2e = {...type_effectiveness[t2]};
        let t1e = {...type_effectiveness[t1]};

        // if not dual typing
        if ( t1 == t2 ) {
            dual_type = t1e;
            return dual_type;
        };

        // get all types in common and exclusive
        let t1_keys = Object.keys(t1e);
        let t2_keys = Object.keys(t2e);
        let commonD = _.intersection(t1_keys, t2_keys);
        let uniqD = _.xor(t2_keys, t1_keys);
        // combine all unique types into final dual types object
        uniqD.forEach(t => { t1_keys.includes(t) ? dual_type[t] = t1e[t] : dual_type[t] = t2e[t]; });
        // calculate common type effectiveness
        commonD.forEach(t => { dual_type[t] =  t1e[t] * t2e[t]; });
    };

    function typeUpdated(event) {
        // updates variables holding selected types and calls  updateDualType

        t2 = document.getElementById('type2').value;
        t1 = document.getElementById('type1').value;    

        return updateDualType();
    };
</script>

<div id='main-wrapper'>
    <h1>monTyper</h1>
    <h2>Look up a Pokemon's dual type weaknesses.</h2>
    <Selector on:typeUpdate={typeUpdated} />
    <h3>Defending Effectiveness</h3>
    <div id='results'>
        {#await dual_type}
            <p>working...</p>
        {:then dt}}
            <!-- Sort alphabetically -->
            {#each Object.entries(dt).sort(comparer) as [type, effectiveness]}
                <!-- Don't render neutral effectiveness -->
                {#if effectiveness != 1}
                <Card eff={effectiveness} type={type} />
                {/if}
            {/each}
        {/await}
    </div>
</div>