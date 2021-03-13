<script>
    import {writable} from "svelte/store";
    import {tweened} from "svelte/motion";
    import {cubicIn} from "svelte/easing";
    import {fade, scale, slide, fly} from "svelte/transition";
    import Spring from "./Spring.svelte";
    import { flip } from "svelte/animate";

    let boxInput = '';
    let showCards = false;
    let showParagraph = false;

    const progress = tweened(0, {
        delay: 0,
        duration: 700,
        easing: cubicIn
    });

    setTimeout(() => {
        progress.set(0.5);
    }, 1500);

    let boxes = [];

    function addBox() {
        boxes = [boxInput.value, ...boxes];
    }

    function discard(box) {
        boxes = boxes.filter(el => el !== box);
    }
</script>

<h1>Start!</h1>

<progress value={$progress}></progress>

{#if showCards}
    <Spring/>
{/if}

<input type="text" bind:this={boxInput}>
<button on:click={addBox} >Add</button>

{#if showParagraph}
    {#each boxes as box (box)}
        <div transition:fly|local={{delay: 100, duration: 500, x: 100, y: 200}}
            on:click={discard.bind(this, box)}
            on:introstart={() => {console.log('Transition intro start')}}
            on:introend={() => {console.log('Transition intro end')}}
            on:outrostart={() => {console.log('Transition outro start')}}
            on:outroend={() => {console.log('Transition outro end')}}
            animate:flip={{duration:300}}>
            {box}
        </div>
    {/each}
{/if}
<hr>

<button on:click={() => showParagraph = !showParagraph}>Toggle</button>
{#if showParagraph}
    <p in:fade out:fly={{x:300}}>Can you see me?</p>
{/if}
<style>
    div {
        width: 10rem;
        height: 10rem;
        background: #ccc;
        margin: 1rem;
        box-shadow: 0 2px 8px rgba(0,0,0,0.26); 
        border-radius: 5px;
        padding: 1rem;
    }
</style>