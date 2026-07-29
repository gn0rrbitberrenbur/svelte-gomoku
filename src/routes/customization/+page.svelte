<script lang="ts">
  import { Gomoku } from '$lib';
  import '$lib/themes/theme-blue.css';
  import '$lib/themes/theme-pink.css';
  import '$lib/themes/theme-green.css';
</script>

<h1>Demo</h1>
<div class="grid">
  <div class="cell"><Gomoku /></div>
  <div class="cell"><Gomoku class="theme-blue" /></div>
  <div class="cell"><Gomoku class="theme-pink" /></div>
  <div class="cell"><Gomoku class="theme-green" /></div>
</div>

<style>
  .grid {
    display: grid;
    grid-template-columns: repeat(2, auto);
    gap: 2rem;
    justify-content: center;
  }
  .cell {
    display: flex;
    justify-content: center;
  }
</style>