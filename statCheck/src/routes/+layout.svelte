<script>
  import "../app.css";
  import Header from "$lib/HeaderBar.svelte";
</script>

<main class="bg-gray-900 h-screen w-screen p-10 text-white overflow-y-scroll scrollbar-hide">
  <Header />
  <slot />
</main>
