<script>
    import "../app.css";
    import Header from "$lib/HeaderBar.svelte";
    import {Svroller} from "svrollbar";
</script>


<Svroller width="full" height="full">
    <main class="bg-darkBlue-600 h-screen w-screen p-10 text-white overflow-y-scroll scrollbar-hide">
        <Header />
        <slot/>
    </main>
</Svroller>

