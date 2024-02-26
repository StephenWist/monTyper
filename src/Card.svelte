<script>
    let className = 'card';
    export { className as class };
    export let effectiveness = 'Lorem';
</script>

<style>
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

<div class={className}>
    <p>{effectiveness}</p>
    <div></div>
</div>