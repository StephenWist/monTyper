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
        flex-wrap: wrap;
        justify-content: center;
        column-gap: 8px;
        height: 100%;
        width: 100%;
    }

</style>

<script lang='ts'>
    import Card from "../Card.svelte";
    import Selector from "../Selector.svelte";
    import _ from 'lodash';
    
    let type_effectiveness = {
        'Normal': {'Attacking': {
                        'Rock': 0.5,
                        'Ghost': 0,
                        'Steel': 0.5
                    },
                    'Defending': {
                        'Fighting': 2,
                        'Ghost': 0
                    }
        },
        'Ghost': {'Attacking': {
                    'Normal':0,
                    'Ghost':2,
                    'Steel':0.5,
                    'Psychic':2,
                    'Dark':0.5
                    },
                'Defending': {
                    'Normal':0,
                    'Fighting':0,
                    'Poison':0.5,
                    'Bug':0.5,
                    'Ghost':2,
                    'Dark':2
                }
        },
        'Fighting': {'Attacking': {
                'Normal':2,
                'Flying':0.5,
                'Posion':0.5,
                'Rock':2,
                'Bug':0.5,
                'Ghost':0,
                'Steel':2,
                'Psychic':0.5,
                'Ice':2,
                'Dark':2
            },
            'Defending': {
                'Flying':2,
                'Rock':0.5,
                'Bug':0.5,
                'Psychic':2,
                'Dark':0.5
            }
        },
    };

    let t1 = 'Normal';
    let t2 = 'Normal';
    let dual_type = {'Attacking':{}, 'Defending':{}};

    function typeUpdated(event) {
        // alert(event.detail.selected);
        // alert(event.detail.id);
        if (event.detail.id == 'type1') {
            t1 = event.detail.selected;
        } else {
            t2 = event.detail.selected;
        };

        if ( t1 == t2 ) {
            console.log('Same types, nothing to do.');
            return
        };

        let t2e = {...type_effectiveness[t2]};
        let t1e = {...type_effectiveness[t1]};

        // get all types in common and exclusive
        let t1A_keys = Object.keys(t1e.Attacking);
        let t2A_keys = Object.keys(t2e.Attacking);
        let t1D_keys = Object.keys(t1e.Defending);
        let t2D_keys = Object.keys(t2e.Defending);
        let commonA = _.intersection(t1A_keys, t2A_keys);
        let uniqA = _.xor(t1A_keys, t2A_keys);
        let commonD = _.intersection(t1D_keys, t2D_keys);
        let uniqD = _.xor(t2D_keys, t1D_keys);

        // combine all unique types
        uniqA.forEach(t => { t1A_keys.includes(t) ? dual_type.Attacking[t] = t1e.Attacking[t] : dual_type.Attacking[t] = t2e.Attacking[t]; });
        commonA.forEach(t => { dual_type.Attacking[t] =  t1e.Attacking[t] * t2e.Attacking[t]; });
        uniqD.forEach(t => { t1D_keys.includes(t) ? dual_type.Defending[t] = t1e.Defending[t] : dual_type.Defending[t] = t2e.Defending[t]; });
        commonD.forEach(t => { dual_type.Defending[t] =  t1e.Defending[t] * t2e.Defending[t]; });
        console.log('dual type ', dual_type);
        // sort dual type alphabetically
    };
</script>

<div id='main-wrapper'>
    <h1>Pokemon Gen 2-4 Type Effectiveness Lookup PTEL</h1>
    <Selector on:typeUpdate={typeUpdated} />
    <div id='results'>
        {#if t1 != t2}
        <div id='Attack'>
            {#await dual_type}
                <p>working...</p>
            {:then dt}
                <h3>Attacking</h3>
                {#each Object.keys(dt.Attacking).map((key) => [key, dt.Attacking[key]]) as k}
                    <Card eff={k[1]} type={k[0]} />
                {/each}
            {/await}
        </div>
        <div id='Defend'>
            {#await dual_type}
                <p>working...</p>
            {:then dt}
                <h3>Defending</h3>
                {#each Object.keys(dt.Defending).map((key) => [key, dt.Defending[key]]) as k}
                    <Card eff={k[1]} type={k[0]} />
                {/each}
            {/await}
        </div>
        {/if}
    </div>
</div>