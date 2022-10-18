<script>
    /** @type {import('./$types').PageData} */
    import { page } from '$app/stores';
    import { MATCH_DETAIL } from "../../../urls.ts";

    import StatCard from '$lib/matchDetail/StatCard.svelte'

    let matchId = $page.params.slug;
    let matchData = {};

    // get the data from fetch using the data prop
    const getMatch = async () => {
        const requestOptions = {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ matchId: matchId, season: null })
        };
        const res = await fetch(MATCH_DETAIL, requestOptions);
        const data = await res.json();
        matchData = data;
        return data;
    };

    const getWinner = (result) => {
        if (result === 'Win') {
            return 'WON';
        } else if (result === 'Fail') {
            return 'LOST';
        } else {
            return 'DRAW';
        }
    };

    // make large numbers more readable
    const formatNumber = (num) => {
        // add space every 3 digits
        return num.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ");
    }

</script>

{#await getMatch()}
    <div>
        <p>Loading...</p>
    </div>
{:then data}
    <div>
        <div class="bg-darkBlue-500 flex flex-col px-2 py-3 rounded-lg">
            <div class="flex flex-col gap-1 p-2 border-b-2 border-gray-600">
                <div class="grid auto-cols-max grid-flow-col space-x-4">
                    <div class="text-blue-700 font-bold">
                        <p>BLUE TEAM</p>
                    </div>
                    <div class="w-min">
                        <div class="flex items-center rounded-full  px-3 text-sm font-semibold {getWinner(matchData.matchDetails.blueTeam.result) === 'WON' ? 'bg-teal-800 text-teal-300':'bg-red-800 text-red-300' }">
                            <p>{getWinner(matchData.matchDetails.blueTeam.result)}</p>
                        </div>
                    </div>
                    <div>Tier average - <span class="tracking-widest text-yellow-700">G2</span></div>
                    <div>
                        <div class="text-gray-200 font-bold flex gap-1 mb-1">
                        <p class="text-cyan-400">{matchData.matchDetails.blueTeam.teamStats.kills}</p>
                        /
                        <p class="text-red-700">{matchData.matchDetails.blueTeam.teamStats.deaths}</p>
                        /
                        <p class="text-blue-500">{matchData.matchDetails.blueTeam.teamStats.assists}</p>
                    </div>
                    </div>
                    <div>{formatNumber(matchData.matchDetails.blueTeam.teamStats.dmgChamp)}</div>
                    <div>{matchData.matchDetails.blueTeam.towers}</div>
                </div>
                <div>
                    <StatCard playerData={data.matchDetails.blueTeam.players} />
                </div>
            </div>

            <div class="flex flex-col gap-2 p-2">
                <div class="grid auto-cols-max grid-flow-col space-x-4">
                    <div class="text-red-700 font-bold">
                        <p>RED TEAM</p>
                    </div>
                    <div class="w-min">
                        <div class="flex items-center rounded-full px-3 text-sm font-semibold {getWinner(matchData.matchDetails.redTeam.result) === 'WON' ? 'bg-teal-800 text-teal-300':'bg-red-800 text-red-300'}">
                            <p>{getWinner(matchData.matchDetails.redTeam.result)}</p>
                        </div>
                    </div>
                    <div>Tier average - <span class="tracking-widest text-yellow-700">G4</span></div>
                    <div>
                        <div class="text-gray-200 font-bold flex gap-1 mb-1">
                            <p class="text-cyan-400">{matchData.matchDetails.redTeam.teamStats.kills}</p>
                            /
                            <p class="text-red-700">{matchData.matchDetails.redTeam.teamStats.deaths}</p>
                            /
                            <p class="text-blue-500">{matchData.matchDetails.redTeam.teamStats.assists}</p>
                        </div>
                    </div>
                    <div>{formatNumber(matchData.matchDetails.redTeam.teamStats.dmgChamp)}</div>
                    <div>{matchData.matchDetails.redTeam.towers}</div>
                </div>
                <div>
                    <StatCard playerData={data.matchDetails.redTeam.players} />
                </div>
            </div>
        </div>
    </div>


{:catch error}
    <div>
        <p>{error.message}</p>
    </div>
{/await}