<script>
  import SummName from "$lib/SummName.svelte";
  import ChampionStats from "$lib/ChampionStats.svelte";
  import MatchCard from "$lib/MatchCard.svelte";

  let summonerData = {};

  // fetch data from username
  async function summonerBasic() {
    let myHeaders = new Headers();
    myHeaders.append("Content-Type", "application/json");

    let raw = JSON.stringify({
      "summoner": "homos in paris",
      "region": "oc1"
    });

    let requestOptions = {
      method: 'POST',
      headers: myHeaders,
      body: raw,
      redirect: 'follow'
    };

    const res = await fetch("https://corspog.herokuapp.com/https://api.leaguestats.gg/summoner/basic", requestOptions)
    const data = await res.json()
    console.log(data)

    if (res.ok) {
      summonerData = data.account;
    } else {
      console.log("error")
      throw new Error(data.message)
    }
  }
</script>

<div>
  {#await summonerBasic()}
    <p>loading...</p>
  {:then x}
    <div class="flex flex-row gap-5">
      <div>
        <SummName {...summonerData} />
        <ChampionStats puuid={summonerData.puuid} />
      </div>
      <div class="">
        <MatchCard puuid={summonerData.puuid} accountId={summonerData.accountId} region="oc1" />
      </div>
    </div>
  {:catch error}
    <p>{error.message}</p>
  {/await}
</div>

<style>
  .logo.vite:hover {
    filter: drop-shadow(0 0 2em #747bff);
  }

  .logo.svelte:hover {
    filter: drop-shadow(0 0 2em #ff3e00);
  }
</style>
