<script lang='ts'>
    import AutoComplete from "simple-svelte-autocomplete";
    import { createEventDispatcher, onMount } from 'svelte';
    import { gen, t1, t2 } from './stores';
    const dispatch = createEventDispatcher();
    export let id;
    let selected;
    let gen6 = ['Bug','Dark','Dragon','Electric','Fighting','Fire',
                        'Flying','Ghost', 'Grass','Ground','Ice','Normal',
                        'Poison','Psychic','Rock','Steel','Water','Fairy']
    let gen2_5 = ['Bug','Dark','Dragon','Electric','Fighting','Fire',
                        'Flying','Ghost', 'Grass','Ground','Ice','Normal',
                        'Poison','Psychic','Rock','Steel','Water']
    let gen1 = ['Bug','Dragon','Electric','Fighting','Fire',
                        'Flying','Ghost', 'Grass','Ground','Ice','Normal',
                        'Poison','Psychic','Rock','Water'];

    let types =  {'1':gen1,
                '2-5':gen2_5,
                '6+':gen6
            };

    onMount(() => {
        console.log('types select mounted id: ', {id});
    });

    function typeUpdate() {
        console.log(selected, id);
        if (id === 'type1') {
            $t1 = selected;
        } else {
            $t2 = selected;
        }
        dispatch('typeUpdate', {
            'selected':selected,
            'id':id,
        });
    }

    
 </script>

<style>
    label {
        font-size: 1.2em;
        border: 5px;
        margin: 5px;
        padding: 5px;
    }
    select {
        font-size: 1.2em;
        border: 5px;
        margin: 5px;
        padding: 5px;
        border-radius: 5px;
    }
</style>
<div>
    <label for={id}>
        {#if id === 'type1'}
        Type 1: {:else} Type 2:
        {/if}
    </label>
    <AutoComplete onChange={typeUpdate} items="{types[$gen]}" bind:selectedItem="{selected}" id={id} />
</div>