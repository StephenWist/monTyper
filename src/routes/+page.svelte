<style lang='scss'>
    * {
        font-family: Bahnschrift, 'DIN Alternate', 'Franklin Gothic Medium', 'Nimbus Sans Narrow', sans-serif-condensed, sans-serif;
        font-weight: normal;   
        text-align:start;
    }

    h1, h2, h3 {
        margin: 8px;
        padding: 8px;
    }
    
    #main-wrapper {
        display: flex;
        flex-direction: column;
        align-items: center;
        font-size: 14px;
        background: rgb(242,232,232);
        background: radial-gradient(circle, rgba(242,232,232,1) 0%, rgba(90,125,124,1) 100%); 
        height: 100vh;
        width: 100vw;
    }

    #results {
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: center;
        align-content: flex-start;
        column-gap: 8px;
        height: 100%;
        max-width: 975px;
    }
</style>

<script lang='ts'>
    import { onMount } from 'svelte';
    import { gen,t1, t2 } from '../stores';
    import _ from 'lodash';
    
    import Card from "../Card.svelte";
    import Selector from "../Selector.svelte";

    // init types
    let gen1TypeEffectiveness = {
        'Normal': { 'Fighting': 2,
                    'Ghost': 0   
        },
        'Ghost': { 'Normal':0,
                    'Fighting':0,
                    'Poison':0.5,
                    'Bug':0.5,
                    'Ghost':2,
                },
        'Fighting': {'Flying':2,
                'Rock':0.5,
                'Bug':0.5,
                'Psychic':2
                },
        'Poison': {'Poison':0.5,
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
            'Fire':0.5,
            'Water':2,
            'Grass':2
        },
        'Bug': {'Fighting':0.5,
            'Flying':2,
            'Ground':0.5,
            'Rock':2,
            'Fire':2,
            'Grass':0.5
        },
        'Fire': {'Ground':2,
            'Rock':2,
            'Bug':0.5,
            'Fire':0.5,
            'Water':2,
            'Grass':0.5,
            'Ice':0.5
        },
        'Water': {'Fire': 0.5,
            'Water': 0.5,
            'Grass':2,
            'Electric':2,
            'Ice':0.5
        },
        'Grass':{'Flying':2,
            'Poison':2,
            'Ground':0.5,
            'Bug':2,
            'Fire':2,
            'Water':0.5,
            'Grass':0.5,
            'Electric':0.5,
            'Ice':2
        },
        'Electric': {'Flying': 0.5,
            'Ground':2,
            'Electric':0.5
        },
        'Psychic': {
            'Ghost':0,
            'Bug':2,
            'Psychic':0.5,
            'Fighting':0.5
        },
        'Ice': {
            'Fighting':2,
            'Rock':2,
            'Fire':2,
            'Ice':0.5,
        },
        'Dragon': {
            'Fire':0.5,
            'Water':0.5,
            'Grass':0.5,
            'Electric':0.5,
            'Ice':2,
            'Dragon':2
        }
    };
    let gen2To5TypeEffectiveness = {
        'Normal': {...gen1TypeEffectiveness.Normal},
        'Ghost': {...gen1TypeEffectiveness.Ghost, 'Dark':2},
        'Fighting': {...gen1TypeEffectiveness.Fighting,'Dark':0.5},
        'Poison': {...gen1TypeEffectiveness.Poison},
        'Flying': {...gen1TypeEffectiveness.Flying},
        'Ground': {...gen1TypeEffectiveness.Ground},
        'Rock': {...gen1TypeEffectiveness.Rock,'Steel':2},
        'Bug': {...gen1TypeEffectiveness.Bug},
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
        'Fire': {...gen1TypeEffectiveness.Fire, 'Steel':0.5},
        'Water': {...gen1TypeEffectiveness.Water, 'Steel': 0.5},
        'Grass': {...gen1TypeEffectiveness.Grass},
        'Electric': {...gen1TypeEffectiveness.Electric,'Steel':0.5},
        'Psychic': {...gen1TypeEffectiveness.Psychic,'Ghost':2,'Dark':2},
        'Ice': {...gen1TypeEffectiveness.Ice,'Steel':2},
        'Dragon': {...gen1TypeEffectiveness.Dragon},
        'Dark': {
            'Fighting':2,
            'Bug':2,
            'Ghost':0.5,
            'Psychic':0,
            'Dark':0.5
        }
    };
    let gen6PlusTypeEffectiveness = {
        'Normal': {...gen1TypeEffectiveness.Normal},
        'Ghost': {...gen2To5TypeEffectiveness.Ghost},
        'Fighting': {...gen2To5TypeEffectiveness.Fighting, 'Fairy':2},
        'Poison': {...gen2To5TypeEffectiveness.Poison, 'Fairy':0.5},
        'Flying': {...gen2To5TypeEffectiveness.Flying},
        'Ground': {...gen2To5TypeEffectiveness.Ground},
        'Rock': {...gen2To5TypeEffectiveness.Rock},
        'Bug': {...gen2To5TypeEffectiveness.Bug},
        'Steel': {...gen2To5TypeEffectiveness.Steel, 'Fairy':0.5},
        'Fire': {...gen2To5TypeEffectiveness.Fire, 'Fairy':0.5},
        'Water': {...gen2To5TypeEffectiveness.Water},
        'Grass': {...gen2To5TypeEffectiveness.Grass},
        'Electric': {...gen2To5TypeEffectiveness.Electric},
        'Psychic': {...gen2To5TypeEffectiveness.Psychic},
        'Ice': {...gen2To5TypeEffectiveness.Ice},
        'Dragon': {...gen2To5TypeEffectiveness.Dragon, 'Fairy':2},
        'Dark': {...gen2To5TypeEffectiveness.Dark, 'Fairy':2},
        'Fairy': {'Fighting':0.5,
                'Poison':2,
                'Bug':0.5,
                'Steel':2,
                'Dragon':0,
                'Dark':0.5
                }
    };
    let allGens = {
        '1': gen1TypeEffectiveness,
        '2-5':gen2To5TypeEffectiveness,
        '6+':gen6PlusTypeEffectiveness,
    };
    let dualType = {};
    let genTypeEffectiveness;
    let illegalGenTypes = false;

    // initial render after TypeSelectors are mounted
    onMount(() => {
        $t1 = 'Bug'; $t2 = 'Bug';
        document.getElementById('type2').value = $t2;
        document.getElementById('type1').value = $t1;
        $gen = '1';
        document.getElementById('genSelect').value = $gen;
        genTypeEffectiveness = allGens[$gen];
        return typeUpdated('onMount');
	});

    $: dualTypesSorted = Object.entries(dualType)
    .sort(comparer)
    .filter(d => d[1] !== 1)


    function genUpdated() {
        genTypeEffectiveness = allGens[$gen];
        return updateDualType();
    };

    // alphabetic sort
    let comparer = function(a,b) {
            if (a[0] < b[0]) return -1;
            if (a[0] > b[0]) return 1;
            return 0;
        };

    function updateDualType() {
        illegalGenTypes = false;
        if ($gen == '1' && ( ['Steel','Dark','Fairy'].includes($t1) || ['Steel','Dark','Fairy'].includes($t2) ) ) {
            illegalGenTypes = true;
        } else if ($gen === '2-5' && ( $t1 === 'Fairy' || $t2 === 'Fairy' ) ) {
            illegalGenTypes = true;
        };
        // calculate dual type weaknesses
        let t2e = {...genTypeEffectiveness[$t2]};
        let t1e = {...genTypeEffectiveness[$t1]};

        // if not dual typing
        if ( $t1 === $t2 ) {
            dualType = t1e;
            return dualType;
        };

        // get all types in common and exclusive
        let t1Keys = Object.keys(t1e);
        let t2Keys = Object.keys(t2e);
        let commonD = _.intersection(t1Keys, t2Keys);
        let uniqD = _.xor(t2Keys, t1Keys);
        // combine all unique types into final dual types object
        uniqD.forEach(t => { t1Keys.includes(t) ? dualType[t] = t1e[t] : dualType[t] = t2e[t]; });
        // calculate common type effectiveness
        commonD.forEach(t => { dualType[t] =  t1e[t] * t2e[t]; });
        console.log("dualtype ", dualType);
        return dualType;
    };

    function typeUpdated() {
        return updateDualType();
    };
</script>

<div id='main-wrapper'>
    <div id='header'>
        <h1>monTyper</h1>
        <h2>Look up a Pokemon's dual type weaknesses.</h2>
    </div>
    <Selector on:genUpdate={genUpdated} on:typeUpdate={typeUpdated} />
    <div id='results-wrapper'>
        <div id='results'>
            {#if !illegalGenTypes && dualTypesSorted?.length}
                {#each dualTypesSorted as [type, effectiveness]}
                    <Card eff={effectiveness} type={type} />
                {/each}
            {:else}
                <h3 style='color:red'>Illegal Generation/Type Combo!</h3>
            {/if}
        </div>
    </div>
</div>