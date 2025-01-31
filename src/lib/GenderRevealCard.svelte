<script lang="ts">
  import { fade } from "svelte/transition";
  import type { Gender } from "./types";
  type Content = Gender | null;

  const GRID_SIZE = 9;
  const ACTUAL_GENDER: Gender = "Boy" as Gender;
  const OTHER_GENDER: Gender = ACTUAL_GENDER === "Boy" ? "Girl" : "Boy";

  let genderGuesses: Content[] = $state(Array(GRID_SIZE).fill(null));
  let { onAllRevealed } = $props();
  let revealedCount: number = $derived(
    genderGuesses.filter((card) => card !== null).length
  );
  let allRevealed: boolean = $derived(revealedCount === GRID_SIZE);

  const cardContents: Content[] = [
    ...Array(Math.floor(GRID_SIZE / 2)).fill(ACTUAL_GENDER),
    ...Array(Math.floor(GRID_SIZE / 2)).fill(OTHER_GENDER),
  ];

  function getNextGuess() {
    const randomIndex = Math.floor(Math.random() * cardContents.length);
    const guess = cardContents[randomIndex];
    cardContents.splice(randomIndex, 1);
    return guess;
  }

  function revealCard(index: number): void {
    if (!genderGuesses[index]) {
      if (revealedCount === GRID_SIZE - 1) {
        genderGuesses[index] = ACTUAL_GENDER;
      } else {
        genderGuesses[index] = getNextGuess();
      }
      genderGuesses = [...genderGuesses];
    }
  }

  $effect(() => {
    if (allRevealed) {
      onAllRevealed(ACTUAL_GENDER);
    }
  });
</script>

<div class="flex flex-col items-center justify-center p-5">
  {#if !allRevealed}
    <h2 class="text-xl md:text-2xl lg:text-4xl font-bold text-white mb-5 text-center">
      Gender Reveal Scratch Card
    </h2>
  {/if}
  <div class="grid grid-cols-3 gap-4 mt-5">
    {#each genderGuesses as genderGuess, index}
      <button
        class="w-24 h-24 sm:w-32 sm:h-32 md:w-40 md:h-40 lg:w-48 lg:h-60 cursor-pointer disabled:cursor-default bg-white shadow-lg rounded-lg flex items-center justify-center text-xl font-semibold text-gray-700"
        class:bg-white={genderGuess === null}
        class:bg-blue-300={genderGuess === "Boy"}
        class:bg-pink-300={genderGuess === "Girl"}
        onclick={() => revealCard(index)}
        disabled={!!genderGuess}
      >
        {#if !!genderGuess}
          <span transition:fade>
            {genderGuess}
          </span>
        {:else}
          Click to reveal
        {/if}
      </button>
    {/each}
  </div>
</div>

<style>
</style>
