<script>
    export let name='GenSelect';
    export let genVal=4;
</script>

<style>
    select {
    font-size: 1.2em;
    padding: 5px;
    margin: 5px 5px;
    border-radius: 5px;
    }
</style>

<select name={name}>
    <option value={genVal}>{genVal}</option>
</select>