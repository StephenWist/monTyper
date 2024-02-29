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

        console.log(t1 + ' Attacking');
        console.log(t1e.Attacking);
        console.log(t2 + 'Attacking');
        console.log(t2e.Attacking);
        // get all types in common
        let commonA = _.intersection(Object.keys(t1e.Attacking), Object.keys(t2e.Attacking));
        // get all unique types
        // let all_types = Object.assign(t1e.Attacking, t2e.Attacking);
        // console.log(commonA);
        // console.log(uniqA);

        commonA.forEach(t => {
            // uniqA[t] = t1e.Attacking[t] * t2e.Attacking[t];
            console.log(t, t1e.Attacking[t], t2e.Attacking[t], t1e.Attacking[t] * t2e.Attacking[t]);
        });
    };
</script>

<div id='main-wrapper'>
    <h1>Pokemon Gen 2-4 Type Effectiveness Lookup PTEL</h1>
    <Selector on:typeUpdate={typeUpdated} />
    <div id='results'>
        {#if t1 != t2}
            {#each [t1] as t}
                {#each [t] as f}
                    <Card class='card se' eff=f />
                {/each}
            {/each}
        {/if}
    </div>
</div>