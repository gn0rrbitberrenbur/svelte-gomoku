<script lang="ts">
  import { Gomoku } from '$lib';
  import '$lib/themes/theme-green.css';

  let game: ReturnType<typeof Gomoku>;
  let locked = $state(false);
  let rowInput = $state(0);
  let colInput = $state(0);
  let awaitingEngine = $state(false);

  function handleMove(m: { row: number; col: number; player: 'black' | 'white' }) {
    if (m.player === 'black') {
      locked = true;
      awaitingEngine = true;
    }
  }

  function submitEngineMove() {
    if (!awaitingEngine) return;
    locked = false;
    const ok = game.move(rowInput, colInput);
    if (ok) {
      awaitingEngine = false;
    } else {
      locked = true;
    }
  }

  function reset() {
    game.reset();
    locked = false;
    awaitingEngine = false;
  }
</script>

<h1>Engine Game (White = Engine)</h1>
<div class="engine">
  <Gomoku bind:this={game} bind:locked class="theme-green" onMove={handleMove} />
  <div class="controls">
    <p>{awaitingEngine ? 'Enter Engine move:' : 'Blacks turn'}</p>
    <label>Row <input type="number" min="0" max="14" bind:value={rowInput} /></label>
    <label>Column <input type="number" min="0" max="14" bind:value={colInput} /></label>
    <button onclick={submitEngineMove} disabled={!awaitingEngine}>Engine-Zug</button>
    <button onclick={reset}>Reset</button>
  </div>
</div>

<style>
  .engine {
    display: flex;
    gap: 2rem;
    justify-content: center;
    align-items: flex-start;
    margin-top: 1rem;
  }
  .controls {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
  }
</style>