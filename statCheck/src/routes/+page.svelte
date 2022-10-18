<script>
    import {writable} from 'svelte/store'
    import {LIVE_MATCH, SUMMONER_BASIC, SUMMONER_OVERVIEW} from "../urls.ts";

    // import skeleton loaders
    import SummonerSkeleton from "$lib/skeletons/SummonerSkeleton.svelte";
    import MatchCardSkeleton from "$lib/skeletons/MatchCardSkeleton.svelte";
    import RecentActivitySkeleton from "$lib/skeletons/RecentActivitySkeleton.svelte";
    import ChampionStatsSkeleton from "$lib/skeletons/ChampionStatsSkeleton.svelte";

    import SummName from "$lib/SummName.svelte";
    import ChampionStats from "$lib/ChampionStats.svelte";
    import MatchCard from "$lib/MatchCard.svelte";
    import RecentActivity from "$lib/recentGames/RecentActivity.svelte";
    import SearchBar from "$lib/SearchBar.svelte";

    let summonerData = {};
    let championData = {};
    let matchData = {};
    let recentMatches = [];
    let errorMessage
    const summoner = writable("Homos in paris")
    const region = writable("oc1")

    // dont think i need this
    const getError = (error) => {
        const messageCodes = {
            404: "Summoner not found",
            403: "API key is invalid",
            429: "Too many requests",
            500: "Internal server error",
            503: "Service unavailable"
        }
        if (messageCodes[error]) {
            errorMessage = messageCodes[error]
        } else {
            errorMessage = "Something went wrong"
        }
    }

    const getSummonerOverview = async (accountId, puuid, region) => {
        const requestOptions = {
            method: 'POST',
            headers: {'Content-Type': 'application/json'},
            body: JSON.stringify({accountId, puuid, region})
        };
        const response = await fetch(SUMMONER_OVERVIEW, requestOptions);
        return await response.json();
    }

    // fetch data from username
    const summonerBasic = async (name, region) => {
        let requestOptions = {
            method: 'POST',
            headers: {'Content-Type': 'application/json'},
            body: JSON.stringify({
                "summoner": name,
                "region": region
            })
        };
        const res = await fetch(SUMMONER_BASIC, requestOptions)
        const data = await res.json()
        if (res.ok) {
            summonerData = data.account;
            recentMatches = data.recentActivity;
            await getSummonerOverview(data.account.accountId, data.account.puuid, region)
                .then(data => {
                    championData = data.stats.champion;
                    console.log(championData)
                    matchData = data.matchesDetails;
                })
                .catch(err => console.log(err))
        } else {
            console.log(res)
            getError(res.status)
        }
    }
</script>

<div>
    <SearchBar bind:value={$summoner} bind:region={$region} />
    {#await summonerBasic($summoner, $region)}
        <div class="flex flex-row gap-5">
            <div>
                <SummonerSkeleton/>
                <ChampionStatsSkeleton/>
                <RecentActivitySkeleton/>
            </div>
            <div class="">
                <MatchCardSkeleton/>
                <MatchCardSkeleton/>
                <MatchCardSkeleton/>
                <MatchCardSkeleton/>
                <MatchCardSkeleton/>
            </div>
        </div>
    {:then x}
        <div class="flex flex-row gap-5">
            <div>
                <SummName {...summonerData}/>
                <ChampionStats championData={championData}/>
                <RecentActivity data={recentMatches}/>
            </div>
            <div class="">
                <MatchCard matchHistory={matchData}/>
            </div>
        </div>
    {:catch e}
        <div class="text-center justify-center">{errorMessage}</div>
    {/await}
</div>
