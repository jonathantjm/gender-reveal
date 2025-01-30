<script lang="ts">
  import GenderRevealCard from "./lib/GenderRevealCard.svelte";
</script>

<main>
  <div class="flex flex-col items-center justify-center min-h-screen bg-gradient-to-r from-pink-300 via-purple-300 to-blue-300">
    <h1 class="text-2xl md:text-4xl lg:text-6xl font-extrabold text-transparent bg-clip-text bg-gradient-to-r from-pink-500 via-purple-500 to-blue-500">
      Jon & Rae's Baby Gender Reveal!
    </h1>

    <div>
      <GenderRevealCard />
    </div>
  </div>
</main>

<style>
</style>
