<script>
    const prompt = `Put these Bruce Willis Movies in order by the year they were
        released, earliest first.`

    let selected_index = null;

    function select_index(index) {
        // you cannot go directly from one selection to another
        if (selected_index === null) {
            selected_index = index;
        } else if (selected_index === index) {
            selected_index = null;
        }
    }

    $: show_arrow = (index) => {
        return !(selected_index === null
            || selected_index === index
            || selected_index === index - 1);
    };

    function click_arrow(move_to_index) {
        console.log('click', move_to_index);
        const without_selected = items.filter((_, index) => index !== selected_index);
        const item_to_move = items[selected_index];
        // when moving down, take account that the item is already removed
        const place_item_at = move_to_index > selected_index ? move_to_index - 1 : move_to_index;

        items = [
            ...without_selected.slice(0, place_item_at),
            item_to_move,
            ...without_selected.slice(place_item_at),
        ];
        console.log(items)

        selected_index = null;
    }

    let items =  [
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
        background: #cbcbcb;
        z-index: 1;
        cursor: pointer;
        max-width: 40px;
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
        border-color: transparent #cbcbcb transparent transparent;
        right: 1.5em;
    }
    .arrow.left::before {
        border-color: transparent transparent transparent #cbcbcb;
        left: 1.5em;
    }
    .no-border {
        border: none;
    }
</style>

<section class='section py-2'>
    <div class=container>
        <h2 class=subtitle>
            {prompt}
        </h2>
    </div>
</section>

<div class=grid-container>
    {#each items as item, index (item.text)}
        {#if show_arrow(index)}
            <div on:click={() => click_arrow(index)}
                class='button arrow left'></div>
        {:else}
            <div></div>
        {/if}


        <button class='button is-rounded is-block work-for-long-text no-border'
            class:is-dark={selected_index === null || selected_index === index}
            disabled={selected_index !== null && selected_index !== index}
            on:click={() => select_index(index)}>
            {item.text}
        </button>

        {#if show_arrow(index)}
            <div on:click={() => click_arrow(index)}
                class='button arrow right'></div>
        {:else}
            <div></div>
        {/if}
    {/each}
        {#if show_arrow(items.length)}
            <div on:click={() => click_arrow(items.length)}
                class='button arrow left'></div>
        {:else}
            <div></div>
        {/if}


        <!-- Empty div as placeholder -->
        <div></div>

        {#if show_arrow(items.length)}
            <div on:click={() => click_arrow(items.length)}
                class='button arrow right'></div>
        {:else}
            <div></div>
        {/if}
</div>
