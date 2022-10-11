<script>
    //https://play.tailwindcss.com/w7pL5olR5c
    export let name;
    export let summonerLevel;
    export let profileIconId;

</script>


<div class="flex flex-row gap-4 p-6 max-w-sm bg-white rounded-lg border dark:bg-gray-900 dark:border-gray-900">
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