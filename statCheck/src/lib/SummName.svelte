<script>
    //https://play.tailwindcss.com/w7pL5olR5c
    import {LIVE_MATCH} from "../urls.ts";
    import LiveGame from "$lib/liveGame/LiveGameBtn.svelte";

    export let name;
    export let summonerLevel;
    export let profileIconId;
    export let id;

    const getLiveGame = async (id, region) => {
        const requestOptions = {
            method: "POST",
            headers: {"Content-Type": "application/json"},
            body: JSON.stringify({id, region})
        }
        const response = await fetch(LIVE_MATCH, requestOptions)

        if (response.ok) {
            return await response.json()
        }
    }
</script>


<div class="flex flex-row gap-4 p-6 max-w-sm rounded-lg bg-darkBlue-600">
    <div class="flex flex-col gap-4">
        <div class="relative flex">
            <img class="border-2 border-yellow-600 w-20 rounded-full" src={"https://raw.communitydragon.org/latest/plugins/rcp-be-lol-game-data/global/default/v1/profile-icons/" + profileIconId + ".jpg"} alt="summoner icon"/>
            <div class="absolute border-2 border-yellow-600 top-[60px] left-8 top w-12 h-6 bg-gray-900 rounded-full text-center text-xs font-mono">{summonerLevel}</div>
        </div>
        <div class="relative flex flex-col gap-2">
            <p class="uppercase text-2xl font-semibold">{name}</p>
            <select class="h-8 bg-transparent outline-none">
                <option class="bg-gray-700">ALL</option>
                <option class="bg-gray-700">RANKED SOLO</option>
                <option class="bg-gray-700">NORMAL DRAFT</option>
            </select>
        </div>
    </div>
    {#await getLiveGame(id, 'oc1')}
    {:then data}
        <LiveGame {data}/>
    {/await}
</div>

