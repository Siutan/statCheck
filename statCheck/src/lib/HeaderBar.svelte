<script>
    import {appWindow} from '@tauri-apps/api/window'


</script>

<div data-tauri-drag-region class="h-8 flex justify-end fixed top-0 bottom-0 left-0 right-0 z-50 bg-gradient-to-b bg-darkBlue-500 border-b-2 border-teal-900">

    <div>
        <!-- back button -->
        <div class="h-5 w-5 rounded-full absolute flex left-8 top-1 bg-transparent border-none outline-none hover:bg-teal-500 items-center" on:click={() => history.back()}>
            <svg class="h-4 w-4 mx-auto text-white" fill="none" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                viewBox="0 0 24 24" stroke="currentColor">
                <path d="M10 19l-7-7m0 0l7-7m-7 7h18"></path>
            </svg>
        </div>
    </div>

    <div class="">
        <!-- back button -->
        <div class="h-5 w-5 rounded-full absolute flex left-14 top-1 bg-transparent border-none outline-none hover:bg-teal-500 items-center" on:click={() => history.forward()}>
            <svg class="h-4 w-4 mx-auto text-white" fill="none" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                viewBox="0 0 24 24" stroke="currentColor">
                <path d="M14 5l7 7m0 0l-7 7m7-7H3"></path>
            </svg>
        </div>
    </div>


    <div
            class="titlebar-button m-1 rounded-full hover:bg-teal-600"
            id="titlebar-minimize"
            on:click={() => appWindow.minimize()}
    >
<!-- minimize svg -->
        <svg
                xmlns="http://www.w3.org/2000/svg"
                width="10"
                height="1"
                viewBox="0 0 10 1"
        >
            <rect
                    width="10"
                    height="1"
                    fill="#fff"
            />
        </svg>
    </div>
    <div
            class="titlebar-button m-1 rounded-full hover:bg-teal-600"
            id="titlebar-maximize"
            on:click={() => appWindow.toggleMaximize()}
    >
        <!-- maximize svg mac style -->
        <svg
                xmlns="http://www.w3.org/2000/svg"
                width="10"
                height="10"
                viewBox="0 0 10 10"
        >
            <rect
                    width="10"
                    height="10"
                    fill="#fff"
            />
            <rect
                    x="1"
                    y="1"
                    width="8"
                    height="8"
            />
        </svg>
    </div>
    <div
            class="titlebar-button m-1 rounded-full hover:bg-red-900"
            id="titlebar-close"
            on:click={() => appWindow.close()}
    >
<!-- close svg -->
        <svg
                xmlns="http://www.w3.org/2000/svg"
                width="10"
                height="10"
                viewBox="0 0 10 10"
        >
            <path
                    d="M 1 1 L 9 9 M 9 1 L 1 9"
                    stroke="#fff"
                    stroke-width="2"
            />
        </svg>
    </div>
</div>

<style>
    .titlebar-button {
        display: inline-flex;
        justify-content: center;
        align-items: center;
        width: 20px;
        height: 20px;
    }

</style>