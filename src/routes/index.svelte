<script>
	  import { flip } from 'svelte/animate';
    import {cubicInOut, cubicOut, cubicIn} from 'svelte/easing';

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

    function move_selected_to(move_to_index) {
        if (!show_arrow(move_to_index)) {
            return;
        }
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

    .flex-column {
        display: flex;
        flex-direction: column;
    }

    .invisible {
        border: none;
        background: transparent;
    }
    .with-margin {
        margin: 0 .5em;
    }
</style>

<section class='section py-2'>
    <div class=container>
        <h2 class=subtitle>
            {prompt}
        </h2>
    </div>
</section>

<div class='flex-column with-margin'>
    {#each items as item, index (item.text)}
        <div class=flex-column
            animate:flip={{duration: 700, easing: cubicInOut}}>

            <div class='button is-rounded invisible'
                class:is-hovered={show_arrow(index)}
                on:click={() => move_selected_to(index)}
                hidden={!show_arrow(index)}></div>

            <button
                class='button is-rounded work-for-long-text m-0'
                disabled={selected_index !== null && selected_index !== index}
                on:click={() => select_index(index)}>
                {item.text}
            </button>
        </div>

    {/each}

    <div class='button m-0 is-rounded invisible'
        class:is-hovered={show_arrow(items.length)}
        on:click={() => move_selected_to(items.length)}
        hidden={!show_arrow(items.length)}></div>
</div>
