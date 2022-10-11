<script>
    import {appWindow} from '@tauri-apps/api/window'


</script>

<div data-tauri-drag-region class="h-6 flex justify-end fixed top-0 bottom-0 left-0 right-0 z-50">

    <div class="p-1 place-items-end">
        <input class="bg-gray-800  shadow-2xl h-5 top-5" type="search" />
    </div>

    <div
            class="titlebar-button hover:bg-gray-800"
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
            class="titlebar-button hover:bg-gray-800"
            id="titlebar-maximize"
            on:click={() => appWindow.toggleMaximize()}
    >
        <!-- maximize svg -->
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
                    x="2"
                    y="2"
                    width="6"
                    height="6"
                    fill="#000"
            />
        </svg>
    </div>
    <div
            class="titlebar-button hover:bg-gray-800"
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
        width: 30px;
        height: 30px;
    }

</style>