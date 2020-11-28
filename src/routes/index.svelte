<script>
    const prompt = `Put these Bruce Willis Movies in order by the year they were
        released, earliest first.`

    const items =  [
        {text: "The Fifth Element", value: 1997},
        {text: "Die Hard", value: 1988},
        {text: "Pulp Fiction", value: 1994},
        {text: "Death Becomes Her", value: 1992},
        {text: "The Sixth Sense", value: 1999},
        {text: "Dr. Strangelove or: How I Learned to Stop Worrying and Love the Bomb", value: 1995},
        {text: "12 Monkeys", value: 1995},
    ];
</script>
<style>
    .is-column {
        display: flex;
        flex-direction: column;
        flex-basis: auto;
        flex-wrap: nowrap;
        align-items: center;
    }
    .work-for-long-text {
        height: auto;
        white-space: normal;
    }
    .button::before {

    }
    .floater {

    }
    .holder {

    }


    .grid-container {
        display: grid;
        grid-template-columns: 1fr 10fr 1fr;
        row-gap: 1.5em;
        column-gap: 0em;
        margin-top: 2.3em;
    }
    .plus {
        top: -2.3em;
        position: relative;
        height: 2.5em;
    }
    .plus::before {
        width: 0;
        height: 0;
        border-width: 1.5em 4em;
        border-style: solid;
        content: '';
        position: absolute;
    }
    .right::before {
        border-color: transparent rgb(217, 255, 0) transparent transparent;
        right: 0em;
    }
    .left::before {
        border-color: transparent transparent transparent rgb(217, 255, 0);
        left: 0em;
    }
</style>

<div class=grid-container>
    {#each items as {text}}
        <div class='plus left'></div>

        <button class='button is-rounded is-block work-for-long-text'>
            {text}
        </button>

        <div class='plus right'></div>
    {/each}
        <div class='plus left'></div>
        <div></div>
        <div class='plus right'></div>
</div>

<div class='container'>
    <div class='notification'>
        {prompt}
    </div>
    <div class='is-column'>
        {#each items as {text}}
            <div class=holder>
            <button class='button is-rounded is-block my-2 work-for-long-text'>
                {text}
            </button>
            <div class=floater>
                foo
            </div>
            </div>
        {/each}
    </div>
</div>
