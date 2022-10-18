<script>

    export let matchHistory

    const matchResult = (result) => {
        return result === "Win";
    }

    const getGameMode = (gameMode) => {
        switch (gameMode) {
            case 400:
                return "Draft Pick";
            case 420:
                return "Ranked Solo/Duo";
            case 430:
                return "Blind Pick";
            case 440:
                return "Ranked Flex";
            case 450:
                return "ARAM";
            case 830:
                return "Co-op vs AI Intro";
            case 840:
                return "Co-op vs AI Beginner";
            case 850:
                return "Co-op vs AI Intermediate";
            case 1020:
                return "One for All";
            case 1200:
                return "Nexus Blitz";
            case 1300:
                return "Nexus Blitz";
            case 1900:
                return "URF";
            case 2000:
                return "Tutorial";
            case 2010:
                return "Snowdown Showdown";
            case 2020:
                return "One for All";
            default:
                return "Unknown";
        }
    }

    const getPosition = (role) => {
        role = role.toLowerCase()
        if (role === "none") {
            return 0
        }
        return `https://raw.communitydragon.org/latest/plugins/rcp-fe-lol-clash/global/default/assets/images/position-selector/positions/icon-position-${role}-blue.png`
    }

    const getItem = (item) => {
        if (item !== null) {
            return item.image;
        }
        // return black image if no item
        return "data:image/gif;base64,R0lGODlhAQABAAD/ACwAAAAAAQABAAACADs="
    }

    const convertThousands = (stat) => {
        // return gold in k if over 1000
        if (stat > 1000) {
            return `${(stat / 1000).toFixed(1)}K`
        }
    }

    const secondsToMinutes = (seconds) => {
        let minutes = Math.floor(seconds / 60);
        let secondsLeft = seconds % 60;
        if (secondsLeft < 10) {
            secondsLeft = `0${secondsLeft}`
        }
        return `${minutes}:${secondsLeft}`
    }

    const getDate = (date) => {
        // convert epoch to date
        let d = new Date(date)
        // if date is today, return time
        if (d.toDateString() === new Date().toDateString()) {
            return d.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })
        }
        // if date is yesterday, return yesterday
        if (d.toDateString() === new Date(new Date().setDate(new Date().getDate() - 1)).toDateString()) {
            return "Yesterday"
        }
        // if date is not today or yesterday, return date
        return d.toLocaleDateString([], { month: 'short', day: 'numeric' })
    }

</script>

{#each matchHistory as match}

    <div class="relative bg-darkBlue-500 my-5 px-5 py-5 shadow-xl ring-1 ring-gray-900/5 sm:mx-auto sm:rounded-lg sm:px-8 cursor-pointer">
        <a href={"/match/" + match.matchId} class="relative mx-auto">
            <div class="grid grid-cols-6 gap">
                <!-- Champion Icon and Lane -->
                <div class=" w-20 h-20 overflow-hidden border-solid border-2 border-teal-600 rounded-lg">
                    <img src={match.champion.icon} alt="Champion Icon"/>
                    <img class="absolute w-8 inset-14 bg-gray-900 rounded-md {!getPosition(match.role) ? 'hidden' : 'block'  }" src={getPosition(match.role)} alt="Lane Icon"/>
                </div>
                <!-- Outcome and Gamemode -->
                <div class="pl-5 pt-3 text-center">
                    <p class='{matchResult(match.result) ? "text-teal-500":"text-red-500"} font-extrabold'> {matchResult(match.result) ? "VICTORY" : "DEFEAT"} </p>
                    <p class="text-gray-500 font-semibold">{getGameMode(match.gamemode)}</p>
                </div>
                <!-- KDA -->
                <div class="pl-5 pt-3 text-center">
                    <div class="text-gray-200 font-bold flex gap-1 mb-1">
                        <p class="text-cyan-400">{match.stats.kills}</p>
                        /
                        <p class="text-red-700">{match.stats.deaths}</p>
                        /
                        <p class="text-blue-500">{match.stats.assists}</p>
                    </div>
                    <div class="flex self-center text-gray-400 text-xs font-thin">{match.stats.kda} KDA</div>
                </div>
                <!-- Items -->
                <div class=" w-24 h-20 grid grid-cols-3 grid-rows-2 gap-x-1 gap-y-0">
                    {#each match.items as item}
                        <img class="rounded-lg" src={getItem(item)} alt="Item Icon"/>
                    {/each}
                </div>
                <!-- CS + Gold + DMG + KP -->
                <div class="pl-5 text-left text-xs font-bold grid grid-cols-1 grid-rows-4">
                    <p class="text-green-500">{match.stats.minions} CS</p>
                    <p class="text-yellow-600">{convertThousands(match.stats.gold)}</p>
                    <p class="text-red-800">{convertThousands(match.stats.dmgChamp)} DMG</p>
                    <p class="text-purple-700">{match.stats.kp}% KP</p>
                </div>
                <!-- Gametime -->
                <div class="pt-3 text-gray-200 text-center text-lg font-semibold">
                    <p>{getDate(match.date)}</p>
                    <p>{secondsToMinutes(match.time)}</p>
                </div>
            </div>
        </a>
    </div>

{/each}