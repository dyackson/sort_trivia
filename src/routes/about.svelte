<svelte:head>
	<title>About</title>
</svelte:head>
<script>
    import {onMount, tick} from 'svelte';
    let i = 0;

    function makeDatum() {
        return {
            id: i++,
            text: 'Donec neque quam, dignissim in, mollis nec, sagittis eu, wisi.',
        };
    }

    let data = Array(7).fill(null).map(makeDatum);

    let fetching = false;

    async function getNext5() {
        if (fetching) {
            return;
        } else {
            fetching = true;
            // await tick();
            await new Promise(r => setTimeout(r, 3000));
            const more = Array(5).fill(null).map(makeDatum);
            data = [...data, ...more];
            fetching = false;
        }
    }

    let scrollable;

    function onScroll(event) {
        const element = event.target;
        console.log({
            scrollHeight: element.scrollHeight,
            scrollTop: element.scrollTop,
            clientHeight: element.clientHeight,
        });
        if (Math.abs(element.scrollHeight - element.scrollTop - element.clientHeight) <= 3.0) {
            console.log('scrolled');
            getNext5();
        }
    }

    // Returns a function, that, as long as it continues to be invoked, will not
    // be triggered. The function will be called after it stops being called for
    // N milliseconds. If `immediate` is passed, trigger the function on the
    // leading edge, instead of the trailing.
    function debounce(func, wait, immediate) {
	      var timeout;
	      return function() {
		        var context = this, args = arguments;
		        var later = function() {
			          timeout = null;
			          if (!immediate) func.apply(context, args);
		        };
		        var callNow = immediate && !timeout;
		        clearTimeout(timeout);
		        timeout = setTimeout(later, wait);
		        if (callNow) func.apply(context, args);
	      };
    }
</script>

<style>
    .scrollable {
        height: 90vh;
        overflow: auto;
    }
</style>

<div class=scrollable on:scroll={onScroll} bind:this={scrollable}>
{#each data as datum (datum.id)}
    <div class='tile box is-primary'><p>{datum.id}: {datum.text}</p></div>
{/each}

{#if fetching}
    ...fetching...
{:else}
    scroll down to fetch more
    <!-- <button class=button on:click={getNext5}>Add 5</button> -->
{/if}
</div>
