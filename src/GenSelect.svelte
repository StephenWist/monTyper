<script>
    export let name='GenSelect';
    export let genVal='2-4';
</script>

<style>
    label {
        font-size:large;
    }
    select {
    font-size: 1.2em;
    padding: 5px;
    border: 5px;
    margin: 5px 5px;
    border-radius: 5px;
    }
</style>

<label for='op'>Generation:</label>
<select name={name}>
    <option id='op' value={genVal}>{genVal}</option>
</select>