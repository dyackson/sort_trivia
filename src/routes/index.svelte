<script>
	  import { flip } from 'svelte/animate';
    import {cubicInOut, cubicOut, cubicIn} from 'svelte/easing';

    const prompt = `Put these Bruce Willis Movies in order by the year they were
        released, earliest first.`

    let items =  [
        {text: "The Fifth Element", value: 1997},
        {text: "Die Hard", value: 1988},
        {text: "Pulp Fiction", value: 1994},
        {text: "Death Becomes Her", value: 1992},
        {text: "The Sixth Sense", value: 1999},
        {text: "Dr. Strangelove or: How I Learned to Stop Worrying and Love the Bomb", value: 1995},
        {text: "12 Monkeys", value: 1995},
    ];

    let selected_index = null;

    // higher-indexed items are atop the z-axis. when an the selected item moves
    // in the list, its list index is immediately changed, even though it's still
    // animating. we always want it to have the highest z value, even if moving
    // up the list, so we give the destination list index a high z value.
    let destination_index = null;

    function select_index(index) {
        // you cannot go directly from one selection to another
        if (selected_index === null) {
            selected_index = index;
        } else if (selected_index === index) {
            selected_index = null;
        }
    }

    function can_move_to(index) {
        return !(selected_index === null
            || selected_index === index
            || selected_index === index - 1);
    };

    function move_selected_to(move_to_index) {
        if (!can_move_to(move_to_index)) {
            return;
        }

        const without_selected = items.filter((_, index) => index !== selected_index);
        const item_to_move = items[selected_index];

        // when moving down, take account that the item is already removed
        const place_item_at = move_to_index > selected_index ?
            move_to_index - 1 : move_to_index;

        // Make the selected item move in front of the others.
        destination_index = place_item_at;

        items = [
            ...without_selected.slice(0, place_item_at),
            item_to_move,
            ...without_selected.slice(place_item_at),
        ];

        selected_index = null;
    }

    let showing_answer = false;

    // for stashing the user's answer so that can compare to the correct by toggling
    let last_user_order = [];

    function toggle_answer() {
        showing_answer = !showing_answer;

        if (showing_answer) {
            // revealing answer
            last_user_order = [...items];
            items = [...items].sort((a, b) => a.value - b.value);
        } else {
            // revert to last user answer
            items = last_user_order;
        }
    }

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

    .top {
        z-index: 9999;
    }
</style>


<section class='section py-2'>
    <div class=container>
        <div class=subtitle>
            {prompt}
        </div>
    </div>
</section>

<div class='flex-column with-margin'>
    {#each items as item, index (item.text)}
        <div class=flex-column
            class:top={index === destination_index}
            animate:flip={{duration: 700, easing: cubicInOut}}>

            <div class='button invisible'
                on:click={() => move_selected_to(index)}
                ></div>

            <button
                class='button is-rounded work-for-long-text m-0'
                disabled={selected_index !== null && selected_index !== index}
                on:click={() => select_index(index)}>
                {item.text}
                {#if showing_answer}
                    ({item.value})
                {/if}

            </button>
        </div>

    {/each}

    <div class='button m-0 invisible'
        on:click={() => move_selected_to(items.length)}
        ></div>

    <div class='button is-info' on:click={toggle_answer}>
        {#if showing_answer}
            Change back to how I had it
        {:else}
            Show correct order
        {/if}
    </div>
</div>
