<script lang="ts">
  import GenderRevealCard from "./lib/GenderRevealCard.svelte";
  import type { Gender } from "./lib/types";

  let displayOptions = $state({
    backgroundColor:
      "bg-gradient-to-r from-pink-300 via-purple-300 to-blue-300",
    title: "Jon & Rae's Baby Gender Reveal!",
    titleColour: "bg-gradient-to-r from-pink-500 via-purple-500 to-blue-500"
  });

  function setDisplayOptionsByGender(gender: Gender) {
    displayOptions.title = `It's a ${gender}!`;
    if (gender === "Boy") {
      displayOptions.backgroundColor = "bg-blue-200";
      displayOptions.titleColour = "bg-blue-400";
    } else {
      displayOptions.backgroundColor = "bg-pink-200";
      displayOptions.titleColour = "bg-pink-400";
    }
  }
</script>

<main>
  <div
    class={`flex flex-col items-center justify-center min-h-screen ${displayOptions.backgroundColor}`}
  >
    <h1
      class={`text-2xl md:text-4xl lg:text-6xl font-extrabold text-transparent bg-clip-text ${displayOptions.titleColour}`}
    >
      {displayOptions.title}
    </h1>

    <div>
      <GenderRevealCard onAllRevealed={setDisplayOptionsByGender} />
    </div>
  </div>
</main>

<style>
</style>
