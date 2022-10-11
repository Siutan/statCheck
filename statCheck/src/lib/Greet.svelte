<script lang="ts">
  import { invoke } from "@tauri-apps/api/tauri";

  let name = "";
  let greetMsg = "";

  async function greet() {
    // Learn more about Tauri commands at https://tauri.app/v1/guides/features/command
    greetMsg = await invoke("greet", { name });
  }
</script>

<div>
  <div class="row">
    <input class="bg-gray-900" id="greet-input" placeholder="Enter a name..." bind:value={name} />
    <button class="bg-gray-900" on:click={greet}> Greet </button>
  </div>
  <p>{greetMsg}</p>
</div>
