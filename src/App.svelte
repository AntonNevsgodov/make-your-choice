<script lang="ts">
  import { scale } from 'svelte/transition';
  import SunIcon from './SunIcon.svelte';

  let initialChoice: string | null = null;
  let currentChoice: string | null = null;

  function makeChoice(choice: string) {
    if (currentChoice) return;
    initialChoice ||= choice;
    currentChoice = choice;
    setTimeout(() => currentChoice = null, 2000);
  }

  const question = decodeURIComponent(location.hash.slice(1)) || 'Do you like chocolate ice cream?';
  if (!location.hash) location.hash = question;
</script>

<main>
  {#if !currentChoice}
    <div
      class="stack-item question-container"
      out:scale={{ opacity: 0, duration: 100, start: 0.5 }}
      in:scale={{ opacity: 0, duration: 300, start: 0.5 }}
    >
      <h1>{question}</h1>

      <div class="buttons-container">
        <button on:click={() => makeChoice("russian")}>Yes</button>
        <button on:click={() => makeChoice("not-russian")}>No</button>
      </div>
    </div>
  {:else}
    <div
      class="stack-item russian-animation-container"
      out:scale={{ opacity: 0, duration: 200, start: 0.5 }}
      in:scale={{ opacity: 0, duration: 300, start: 0.5 }}
    >
      <div class="sun-icon-container">
        <SunIcon/>
      </div>
      <span>
        {#if currentChoice === initialChoice}
          You're an amazing person!!!
        {:else}
          You're an amazing person too!!!
        {/if}
      </span>
      <div
        class="sun-icon-container"
        style:animation-direction="reverse"
      >
        <SunIcon/>
      </div>
    </div>
  {/if}
</main>

<style>
  h1 {
    width: fit-content;
  }
  main {
    display: grid;
    overflow: hidden;
    height: 100vh;
    width: 100vw;
    place-items: center;
  }
  .stack-item {
    grid-area: 1 / 1 / 2 / 2
  }
  .question-container {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .buttons-container {
    display: flex;
    gap: 10px;
  }
  button {
    display: block;
    width: 50px;
    height: 30px
  }
  .russian-animation-container {
    display: flex;
    gap: 50px;
    align-items: center;
  }
  .sun-icon-container {
    color: gold;
    width: 100px;
    aspect-ratio: 1;
    animation: sun-animation 2s infinite linear;
  }

  @keyframes sun-animation {
    0% {
      scale: 1;
      rotate: 0deg;
    }

    50% {
      scale: 1.3;
      rotate: 180deg;
    }

    100% {
      scale: 1;
      rotate: 360deg;
    }
  }

  span {
    font-size: 48px;
  }
</style>
