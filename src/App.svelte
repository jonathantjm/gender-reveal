<script lang="ts">
  import GenderRevealCard from "./lib/GenderRevealCard.svelte";
  import type { Gender } from "./lib/types";
  import Fireworks from "@fireworks-js/svelte";

  let fireworks: Fireworks;

  let displayOptions = $state({
    backgroundColor:
      "bg-gradient-to-r from-pink-300 via-purple-300 to-blue-300",
    title: "Jon & Rae's Baby Gender Reveal!",
    titleColour: "bg-gradient-to-r from-pink-500 via-purple-500 to-blue-500",
    enableFireworks: false,
    fireworksOptions: {
      opacity: 0.5,
      hue: {
        min: 0,
        max: 360,
      },
      explosion: 10,
      flickering: 0,
      intesity: 40,
      rocketsPoint: {
        min: 40,
        max: 60,
      },
    },
  });

  function setDisplayOptionsByGender(gender: Gender) {
    displayOptions.title = `It's a ${gender}!`;
    displayOptions.enableFireworks = true;
    if (gender === "Boy") {
      displayOptions.backgroundColor = "bg-blue-200";
      displayOptions.titleColour = "bg-blue-400";
      displayOptions.fireworksOptions.hue = {
        min: 175,
        max: 260,
      };
    } else {
      displayOptions.backgroundColor = "bg-pink-200";
      displayOptions.titleColour = "bg-pink-400";
      displayOptions.fireworksOptions.hue = {
        min: 290,
        max: 320,
      };
    }
    const fw = fireworks.fireworksInstance();
    fw.start();
  }

  $effect(() => {
    const fw = fireworks.fireworksInstance();
  });
</script>

<main>
  <div
    class={`flex flex-col items-center justify-center min-h-screen ${displayOptions.backgroundColor}`}
  >
    <h1
      class={`text-2xl sm:text-xl md:text-4xl lg:text-6xl font-extrabold text-transparent bg-clip-text text-center ${displayOptions.titleColour}`}
    >
      {displayOptions.title}
    </h1>

    <div>
      <GenderRevealCard onAllRevealed={setDisplayOptionsByGender} />
    </div>
    {#if displayOptions.enableFireworks}
      <Fireworks
        bind:this={fireworks}
        options={displayOptions.fireworksOptions}
        class="fireworks"
      />
    {/if}
  </div>
</main>

<style>
  :global(.fireworks) {
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    position: fixed;
    background: transparent;
  }
</style>
