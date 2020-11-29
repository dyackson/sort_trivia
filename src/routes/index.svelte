<script>
    const prompt = `Put these Bruce Willis Movies in order by the year they were
        released, earliest first.`

    let selected_index = null;

    function select_index(index) {
        selected_index = selected_index === index
            ? null : index;
    }

    $: show_arrow = (index) => {
        return !(selected_index === null
            || selected_index === index
            || selected_index === index - 1);
    };

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
    .work-for-long-text {
        height: auto;
        white-space: normal;
    }

    .grid-container {
        display: grid;
        grid-template-columns: 1fr 10fr 1fr;
        row-gap: 1.8em;
        column-gap: 0em;
        margin-top: 2.3em;
    }
    .arrow {
        top: -2.3em;
        position: relative;
        height: 2.5em;
        background: rgb(217, 255, 0);
    }
    .arrow::before {
        width: 0;
        height: 0;
        border-width: 1.8em 4em;
        border-style: solid;
        content: '';
        position: absolute;
        top: -0.5em;
    }
    .arrow.right::before {
        border-color: transparent rgb(217, 255, 0) transparent transparent;
        right: 1.5em;
    }
    .arrow.left::before {
        border-color: transparent transparent transparent rgb(217, 255, 0);
        left: 1.5em;
    }
    .no-border {
        border: none;
    }
</style>

<div class=grid-container>
    {#each items as item, index (item.text)}
        <div class={show_arrow(index) && 'arrow left'}></div>

        <button class='button is-rounded is-block work-for-long-text no-border'
            class:is-primary={selected_index === null || selected_index === index}
            on:click={() => select_index(index)}>
            {item.text}
        </button>

        <div class={show_arrow(index) && 'arrow right'}></div>
    {/each}
        <div class={show_arrow(items.length) && 'arrow left'}></div>
        <div></div>
        <div class={show_arrow(items.length) && 'arrow right'}></div>
</div>
