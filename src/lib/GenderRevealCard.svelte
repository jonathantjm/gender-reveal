<script lang="ts">
  import { fade } from "svelte/transition";

  type Gender = "Boy" | "Girl";
  type Content = Gender | null;
  type Card = {
    revealed: boolean;
    content: Content;
  };
  const gridSize = 9;

  let actualGender: Gender = "Boy";
  const otherGender: Gender = actualGender === "Boy" ? "Girl" : "Boy";
  let cards: Card[] = $state(
    Array(gridSize).fill({
      revealed: false,
      content: null,
    })
  );
  let revealedCount: number = $derived(
    cards.filter((card) => card.revealed).length
  );
  let allRevealed: boolean = $derived(revealedCount === gridSize);

  const cardContents: Content[] = [
    ...Array(Math.floor(gridSize / 2)).fill(actualGender),
    ...Array(Math.floor(gridSize / 2)).fill(otherGender),
  ];

  function getGuess() {
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
        cards[index].content = actualGender;
      } else {
        cards[index].content = getGuess();
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
    <p transition:fade>Congratulations! It's a {actualGender}!</p>
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
