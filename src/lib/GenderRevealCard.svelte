<script lang="ts">
  import { fade } from "svelte/transition";

  type Gender = "Boy" | "Girl";
  type Content = Gender | null;
  type Card = {
    revealed: boolean;
    content: Content;
  };
  const GRID_SIZE = 9;
  const ACTUAL_GENDER: Gender = "Boy";
  const OTHER_GENDER: Gender = ACTUAL_GENDER === "Boy" ? "Girl" : "Boy";

  let cards: Card[] = $state(
    Array(GRID_SIZE).fill({
      revealed: false,
      content: null,
    })
  );
  let revealedCount: number = $derived(
    cards.filter((card) => card.revealed).length
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
    if (!cards[index].revealed) {
      cards[index].revealed = true;

      // If this is the last card, ensure it shows the actual gender
      if (revealedCount === 9) {
        cards[index].content = ACTUAL_GENDER;
      } else {
        cards[index].content = getNextGuess();
      }
    }
  }
</script>

<div class="gender-reveal-card">
  <h2>Gender Reveal Scratch Card</h2>
  <div class="card-grid">
    {#each cards as card, index}
      <button
        class="card"
        onclick={() => revealCard(index)}
        disabled={card.revealed}
      >
        {#if card.revealed}
          <span transition:fade>
            {card.content}
          </span>
        {:else}
          Click to reveal
        {/if}
      </button>
    {/each}
  </div>
  {#if allRevealed}
    <p transition:fade>Congratulations! It's a {ACTUAL_GENDER}!</p>
  {/if}
</div>

<style>
  .gender-reveal-card {
    text-align: center;
  }

  .card-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 10px;
    margin-top: 20px;
  }

  .card {
    aspect-ratio: 1;
    font-size: 18px;
    border: 2px solid #ccc;
    background-color: #999999;
    cursor: pointer;
  }

  .card:disabled {
    cursor: default;
  }
</style>
