<script>
    //https://play.tailwindcss.com/QphOWy5B13?layout=horizontal

    export let playerData

    console.log(playerData)

    const setStatColor = (stat, threshold) => {
        if (stat >= threshold) {
            return 'text-teal-600'
        } else {
            return 'text-red-600'
        }
    }
    // make large numbers more readable
    const formatNumber = (num) => {
        // add space every 3 digits
        return num.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ");
    }

    // get rank if exists
    const getRank = (rank) => {
        const soloRank = rank[420]
        const flexRank = rank[440]
        if (rank[420]) {
            return soloRank.shortName
        } else if (rank[440]) {
            return flexRank.shortName
        } else {
            return 'Unranked'
        }
    }
</script>

{#each playerData as player}

    <div class="relative bg-darkBlue-500 px-8 py-2 ring-1 ring-gray-900/5 sm:mx-auto sm:rounded-lg sm:px-8 hover:bg-gray-800">
        <div class="relative mx-auto">
            <div class="grid grid-flow-col auto-cols-max gap-2 justify-between">
                <!-- Champion Icon and Lane -->
                <div class="h-16 w-16 overflow-hidden rounded-lg border-2 border-solid border-teal-500">
                    <img src={player.champion.icon} alt="Champion Icon"/>
                </div>
                <!-- Outcome and Gamemode -->
                <div class="w-28 self-center text-left">
                    <p class="font-extrabold text-sm text-teal-500 overflow-ellipsis">{player.name}</p>
                    {#if player.rank}
                        <p class="text-gray-400">{getRank(player.rank)}</p>
                    {/if}
                </div>
                <!-- KDA -->
                <div class="pl-5 pt-3 w-28 text-center">
                    <div class="text-gray-200 font-bold flex gap-1 mb-1">
                        <p class="text-cyan-400">{player.stats.kills}</p>
                        /
                        <p class="text-red-700">{player.stats.deaths}</p>
                        /
                        <p class="text-blue-500">{player.stats.assists}</p>
                    </div>
                    <p class="flex text-xs font-thin text-gray-400">{player.stats.kda} KDA</p>
                </div>
                <div class="self-center px-2 w-28">
                    <p class="font-semibold text-sm">{player.stats.minions} (<span
                            class="{setStatColor(player.percentStats.minions, 7.6)}">{player.percentStats.minions}</span>)
                        CS</p>
                </div>
                <div class="self-center px-2 w-28">
                    <p class="font-semibold"><span
                            class="pr-1 {setStatColor((player.stats.kp).replace('%', ''), 50)}">{player.stats.kp}</span>KP
                    </p>
                    <!-- vision score -->
                    {#if player.stats.vision > 0}
                        <div class="flex flex-row gap-1">
                            <p class="font-semibold">{player.stats.vision} (
                                <span
                                        class="{setStatColor(player.percentStats.vision, 1)}">
                                {player.percentStats.vision}
                            </span>)
                            </p>
                            <svg
                                    class="mt-1"
                                    xmlns="http://www.w3.org/2000/svg" width="16" height="16"><g fill="none"><path
                                    fill="#FFF"
                                    d="M8.516 5.562L9.92 13.81 8 15.51l-1.92-1.7 1.404-8.246.52.59zM6.08 0h3.84l.64 1.108L8.007 4.43 5.44 1.108z"/><path
                                    fill="#8890B5"
                                    d="M0 2.954h5.151l1.441 1.624-.96 4.062-2.423-1.255 1.24-2.142c-.98 0-1.589-.025-1.827-.074-.358-.074-.8-.312-1.494-.93A6.458 6.458 0 010 2.953zm16 0a6.458 6.458 0 01-1.128 1.284c-.694.62-1.136.857-1.494.931-.238.05-.847.074-1.826.074l1.239 2.142-2.423 1.255-.96-4.062 1.44-1.624H16z"/></g>
                            </svg>
                        </div>
                    {/if}
                </div>
                <div class="self-center w-40 px-2 col-span-4">
                    <p class="font-semibold text-sm px-1">{formatNumber(player.stats.dmgChamp)} (<span class="{setStatColor((player.percentStats.dmgChamp).replace('%', ''), 20)}">{player.percentStats.dmgChamp}</span>) DMG</p>
                    <div class="h-2.5 w-full rounded-full bg-gray-200 dark:bg-gray-700">
                        <div class="h-2.5 rounded-full bg-teal-600" style="width: {player.percentStats.dmgChamp}"></div>
                    </div>
                </div>
                <!-- Items -->
                <div class="grid w-60 grid-cols-6 gap-x-2 gap-y-0 self-center">
                    {#each player.items as item}
                        {#if item !== null}
                            <img class="rounded-lg border border-teal-600" src={item.image} alt={item.name}/>
                        {:else }
                            <div class="rounded-lg bg-gray-900 border border-teal-600"></div>
                        {/if}
                    {/each}
                </div>
            </div>
        </div>
    </div>

{/each}