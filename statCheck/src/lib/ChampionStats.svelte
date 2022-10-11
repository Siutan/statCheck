<script>
    //https://play.tailwindcss.com/SeutfXb74L
    export let puuid

    let championData = {}

    async function getChampionData() {
        let myHeaders = new Headers();
        myHeaders.append("Content-Type", "application/json");

        let raw = JSON.stringify({
            "puuid": puuid
        });

        let requestOptions = {
            method: 'POST',
            headers: myHeaders,
            body: raw,
            redirect: 'follow'
        };

        const response = await fetch(`https://corspog.herokuapp.com/https://api.leaguestats.gg/summoner/champions`, requestOptions);
        const data = await response.json();

        console.log(data.slice(0, 5));

        if (response.ok) {
            championData = data.slice(0, 5);
        } else {
            console.log("error");
        }
    }
</script>

<div class="py-10 rounded-2xl">
    {#await getChampionData()}
        <div>loading...</div>
    {:then x}
        <table>
            <thead>
            <tr class="border-b bg-gray-50 text-left text-xs font-semibold uppercase tracking-wide text-gray-500 dark:border-gray-700 dark:bg-gray-800 dark:text-gray-400">
                <th class="px-4 py-3 text-teal-500">Champion</th>
                <th class="px-4 py-3 text-teal-500">Played</th>
                <th class="px-4 py-3 text-teal-500">Winrate</th>
                <th class="px-4 py-3 text-teal-500">KDA</th>
            </tr>
            </thead>
            <tbody>
            {#each championData as champion}
                <tr class="text-gray-700 hover:bg-gray-100 dark:text-gray-400 hover:dark:bg-gray-700">
                    <td class="px-4 py-3 flex flex-row gap-2">
                        <div class="relative mr-3 flex gap-2 h-8 w-8 rounded-full">
                            <img class="h-full w-full rounded-full object-cover" src={champion.champion.icon} alt="" loading="lazy" />
                            <div class="absolute inset-0 rounded-full shadow-inner" aria-hidden="true" />
                        </div>
                        <div class="flex">{champion.champion.name}</div>
                    </td>
                    <td class="px-4 py-3 text-sm">{champion.count}</td>
                    <td class="px-4 py-3 text-sm">{Math.round((champion.wins / champion.count) * 100)}%</td>
                    <td class="px-4 py-3 text-xs">{((champion.kills + champion.assists) / champion.deaths).toFixed(2)}</td>
                </tr>
            {/each}
            </tbody>
        </table>
    {:catch error}
        <p>Something went wrong</p>
    {/await}
</div>
