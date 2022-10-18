<script>
    import Tooltip from "$lib/recentGames/dateHover.svelte";

    export let data = [];

    // get the last 90 days and put them in an array
    let date = new Date();
    let dates = [];
    for (let i = 0; i < 110; i++) {
        dates.push(new Date(date.setDate(date.getDate() - 1)).toISOString().split('T')[0]);
    }
    dates.reverse()

    const matchDate = (date) => {
        // loop through the data and find if the date matches
        for (let i = 0; i < data.length; i++) {
            let day = data[i].day.split('T')[0];
            if (day === date) {
                return true
            }
        }
        return false
    }

    const getWinLoss = (date) => {
        // return wins and losses and total games for the date in an array
        for (let i = 0; i < data.length; i++) {
            let day = data[i].day.split('T')[0];
            if (day === date) {
                return [data[i].wins, data[i].losses, data[i].wins + data[i].losses]
            }
        }
    }


    const boxColor = (stats) => {
        // if more wins than losses return green
        if (stats[0] > stats[1]) {
            return 'bg-teal-700';
        }
        // if more losses than wins return red
        if (stats[0] < stats[1]) {
            return 'bg-red-700';
        }
        // if equal wins and losses return yellow
        if (stats[0] === stats[1]) {
            return 'bg-yellow-600';
        }
    }

</script>

<div>
    <div class="m-2 inline-block rounded-lg">
        <div class="p-3 pt-1">
            <div class="flex">
                <span class="ml-12 text-xs font-semibold text-blue-200">Jul</span>
                <span class="ml-16 text-xs font-semibold text-blue-200">Aug</span>
                <span class="ml-16 text-xs font-semibold text-blue-200">Sep</span>
                <span class="ml-16 text-xs font-semibold text-blue-200">Oct</span></div>
            <div class="mt-1 flex">
                <div class="flex flex-col"><span class="text-xs font-semibold leading-snug text-blue-200">Mo</span><span
                        class="mt-1 text-xs font-semibold leading-snug text-blue-200">Tu</span><span
                        class="mt-1 text-xs font-semibold leading-snug text-blue-200">We</span><span
                        class="mt-1 text-xs font-semibold leading-snug text-blue-200">Th</span><span
                        class="mt-1 text-xs font-semibold leading-snug text-blue-200">Fr</span><span
                        class="mt-1 text-xs font-semibold leading-snug text-blue-200">Sa</span><span
                        class="mt-1 text-xs font-semibold leading-snug text-blue-200">Su</span></div>
                <div class="ml-1 flex flex-col flex-wrap" style="width: calc(300px); height: calc(140px);">
                    {#each dates as date}
                        <div>
                            {#if matchDate(date)}
                                <Tooltip date={date} stats={getWinLoss(date)}>
                                    <div aria-haspopup="true">
                                        <div class="ml-1 mt-1 h-4 w-4 cursor-pointer {boxColor(getWinLoss(date))}"></div>
                                    </div>
                                </Tooltip>
                            {:else}
                                <Tooltip date={date}>
                                    <div aria-haspopup="true">
                                        <div class="ml-1 mt-1 h-4 w-4 cursor-pointer bg-gray-700"></div>
                                    </div>
                                </Tooltip>
                            {/if}
                        </div>
                    {/each}
                </div>
            </div>
        </div>
    </div>
</div>