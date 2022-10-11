<script>
    export let puuid
    export let accountId
    export let region

    let matchHistory = {}

    async function getMatchHistory() {
        let myHeaders = new Headers();
        myHeaders.append("Content-Type", "application/json");

        let raw = JSON.stringify({
            "accountId": accountId,
            "puuid": puuid,
            "region": region
        });

        let requestOptions = {
            method: 'POST',
            headers: myHeaders,
            body: raw,
            redirect: 'follow'
        };

        const response = await fetch(`https://corspog.herokuapp.com/https://api.leaguestats.gg/summoner/overview`, requestOptions);
        const data = await response.json()

        if (response.ok) {
            console.log(data.matchesDetails)
            matchHistory = data.matchesDetails
        } else {
            console.log(response.status)
        }
    }

    const matchResult = (match) => {
        return match === "Win";
    }

    const gameMode = (mode) => {
        switch (mode) {
            case 440:
                return "URF"
            case 420:
                return "Ranked"
            case 400:
                return "Normal"
            case 430:
                return "Blind"
            case 450:
                return "ARAM"
            case 700:
                return "Clash"
        }
    }

</script>

{#await getMatchHistory()}
    <p>loading...</p>
{:then x}
    {#if matchHistory}
        {#each matchHistory as match}
            <div class="relative bg-gray-900 my-5 px-5 py-5 shadow-xl ring-1 ring-gray-900/5 sm:mx-auto sm:rounded-lg sm:px-8">
                <div class="relative mx-auto">
                    <div class="grid grid-cols-6 gap">
                        <!-- Champion Icon and Lane -->
                        <div class=" w-20 h-20 overflow-hidden border-solid border-2 border-blue-500 rounded-lg">
                            <img src="https://raw.communitydragon.org/latest/plugins/rcp-be-lol-game-data/global/default/v1/champion-icons/145.png" alt="Champion Icon" />
                            <img class="absolute w-8 inset-14 bg-gray-900 rounded-md" src="https://raw.communitydragon.org/latest/plugins/rcp-fe-lol-clash/global/default/assets/images/position-selector/positions/icon-position-middle.png" alt="Champion Icon" />
                        </div>
                        <!-- Outcome and Gamemode -->
                        <div class="pl-5 pt-3 text-center">
                            <p class='{matchResult(match.result) ? "text-blue-500":"text-red-500"} font-extrabold'> {matchResult(match.result) ? "VICTORY":"DEFEAT"} </p>
                            <p class="text-gray-500 font-semibold">Ranked Flex</p>
                        </div>
                        <!-- KDA -->
                        <div class="pl-5 pt-3 text-center">
                            <p class="text-gray-200 font-bold"> 9/5/27 </p>
                            <p class="inline-block text-gray-400 text-xs font-thin">7.2 KDA</p>
                        </div>
                        <!-- Items -->
                        <div class=" w-24 h-20 grid grid-cols-3 grid-rows-2 gap-x-1 gap-y-0">
                            <img class="rounded-lg" src="https://raw.communitydragon.org/latest/plugins/rcp-be-lol-game-data/global/default/assets/items/icons2d/6672_marksman_t4_behemothslayer.png" alt="Item 1" />
                            <img class="rounded-lg" src="https://raw.communitydragon.org/latest/plugins/rcp-be-lol-game-data/global/default/assets/items/icons2d/3046_marksman_t3_phantomdancer.png" alt="Item 2" />
                            <img class="rounded-lg" src="https://raw.communitydragon.org/latest/plugins/rcp-be-lol-game-data/global/default/assets/items/icons2d/3036_marksman_t3_dominkregards.png" alt="Item 3" />
                            <img class="rounded-lg" src="https://raw.communitydragon.org/latest/plugins/rcp-be-lol-game-data/global/default/assets/items/icons2d/3031_marksman_t3_infinityedge.png" alt="Item 4" />
                            <img class="rounded-lg" src="https://raw.communitydragon.org/latest/plugins/rcp-be-lol-game-data/global/default/assets/items/icons2d/3006_class_t2_berserkersgreaves.png" alt="Item 5" />
                            <img class="rounded-lg" src="https://raw.communitydragon.org/latest/plugins/rcp-be-lol-game-data/global/default/assets/items/icons2d/3026_fighter_t3_guardianangel.png" alt="Item 6" />
                        </div>
                        <!-- CS + Gold + DMG + KP -->
                        <div class="pl-5 text-left text-xs font-bold grid grid-cols-1 grid-rows-4">
                            <p class="text-green-500">225 CS</p>
                            <p class="text-yellow-600">22.1K</p>
                            <p class="text-red-800">12.7K DMG</p>
                            <p class="text-purple-700">72.8% KP</p>
                        </div>
                        <!-- Gametime -->
                        <div class="pt-3 text-gray-500 text-center text-lg font-thin">
                            <p>1d ago</p>
                            <p>44:40</p>
                        </div>
                    </div>
                </div>
            </div>
        {/each}
    {:else}
        <p>no data</p>
    {/if}
{/await}