<script>
    export let date = '';
    export let stats = [0, 0, 0];

    let isHovered = false;
    let x;
    let y;

    const mouseOver = (event) => {
        isHovered = true;
        x = event.pageX + 5;
        y = event.pageY + 5;
    }

    function mouseMove(event) {
        x = event.pageX + 5;
        y = event.pageY + 5;
    }

    const mouseLeave = () => {
        isHovered = false;
    }

</script>

<div
        on:mouseover={mouseOver}
        on:mouseleave={mouseLeave}>
    <slot/>
</div>

{#if isHovered}
    {#if stats[2] > 0}
        <div style="top: {y}px; left: {x}px;" class="absolute p-2 text-white bg-gray-800 text-sm z-50 rounded-lg ">
            <p class="text-teal-500 font-bold">{date}</p>
            <div class="flex gap-2">
                <p class="text-teal-500 font-bold">Wins:</p>
                <p>{stats[0]}</p>
            </div>
            <div class="flex gap-2">
                <p class="text-red-700 font-bold">Loss:</p>
                <p>{stats[2]}</p>
            </div>
        </div>
    {:else}
        <div style="top: {y}px; left: {x}px;" class="absolute p-2 text-white bg-gray-800 text-sm z-50 rounded-b-lg">
            <p class="text-teal-500 font-bold">{date}</p>
            <p>No games played</p>
        </div>
    {/if}

{/if}

<style>
    .tooltip {
        border: 1px solid #ddd;
        box-shadow: 1px 1px 1px #ddd;
        background: white;
        border-radius: 4px;
        padding: 4px;
        position: absolute;
    }
</style>