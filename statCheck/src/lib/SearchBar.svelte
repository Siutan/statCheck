<script>
    export let value = '';
    export let region = '';

    const submitSearch = (event) => {
        // if key is enter
        if (event.keyCode === 13) {
            value = event.target.value
        }
    }

    const changeRegion = (event) => {
        console.log(event.target.value)
        region = event.target.value
    }

    // select region options
    const regions = [
        {value: 'OC1', label: 'OCE'},
        {value: 'EUW1', label: 'EUW'},
        {value: 'EUN1', label: 'EUNE'},
        {value: 'BR1', label: 'BR'},
        {value: 'JP1', label: 'JP'},
        {value: 'KR', label: 'KR'},
        {value: 'LA1', label: 'LAN'},
        {value: 'LA2', label: 'LAS'},
        {value: 'NA1', label: 'NA'},
        {value: 'TR1', label: 'TR'},
        {value: 'RU', label: 'RU'}
    ];
    // <input class="rounded-md bg-gray-800 border-2 border-gray-900 shadow-2xl h-5 text-sm" {value} on:keydown={(e) => {submitSearch(e)}}/>
</script>

<div class="absolute top-1 right-24 z-50 ">

    <div class="flex justify-center">
        <div class="mb-3 xl:w-96">
            <div class="input-group relative flex flex-row items-stretch w-full ">
                <input type="search"
                       class="h-5 rounded-l-lg rounded-r-none relative flex-auto w-full bg-darkBlue-600 bg-clip-padding transition ease-in-out m-0 border-2 border-teal-600 focus:outline-none"
                       placeholder="Search Summoner" aria-label="Search"
                       {value} on:keydown={(e) => {submitSearch(e)}}>
                <select
                        on:change={(e) => {changeRegion(e)}}
                        class="h-[1.45rem] text-white inline-block px-2 bg-teal-600 text-white font-medium text-xs leading-tight uppercase rounded-r-lg shadow-md hover:shadow-lg flex items-center focus:outline-none">
                    {#each regions as region}
                        <option value={region.value}>{region.label}</option>
                    {/each}
                </select>
            </div>
        </div>
    </div>
</div>