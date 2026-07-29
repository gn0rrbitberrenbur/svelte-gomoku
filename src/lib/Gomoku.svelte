<script lang="ts">

  /**
   * Type that represents a player in the game, either 'black' or 'white'.
   */
  type Player = 'black' | 'white';

  /**
   * A move made by a player on the board.
   * @param row (number) - the row index of the move
   * @param col (number) - the column index of the move
   * @param player (Player) - the player who made the move
   * @param move (number) - the sequential number of the move
   */
  type Move = { row: number; col: number; player: Player; move: number };

  let {
    locked = $bindable(false),
    size = $bindable(15),
    showGhost = $bindable(true),
    moveNumber = $bindable(0),
    history = $bindable([]),
    board = $bindable(),
    current = $bindable('black'),
    winner = $bindable(null),
    class: className = '',
    onWin = () => {},
    onMove = () => {},
    onDraw = () => {},
  }: {
    locked?: boolean;
    size?: number;
    showGhost?: boolean;
    moveNumber?: number;
    history?: Move[];
    board?: (Player | null)[][];
    current?: Player;
    winner?: Player | null;
    class?: string;
    onWin?: (player: Player) => void;
    onMove?: (m: { row: number; col: number; player: Player }) => void;
    onDraw?: () => void;
  } = $props();

  /**
   * Create a new empty board of the given size.
   * @returns (Player | null)[][] - a 2D array representing the board
   */
  function createBoard(): (Player | null)[][] {
    return Array.from({ length: size }, () => Array(size).fill(null));
  }

  if (!board) board = createBoard();

  /**
   * Check if the player has won after placing a stone at (r, c).
   * @param r (number) - row index
   * @param c (number) - column index
   * @param player (Player) - the player who placed the stone
   * @returns (boolean) - true if the player has won, false otherwise
   */
  function checkWin(r: number, c: number, player: Player): boolean {
    const b = board!;
    const dirs = [[0, 1], [1, 0], [1, 1], [1, -1]];
    for (const [dr, dc] of dirs) {
      let count = 1;
      for (const s of [1, -1]) {
        let nr = r + dr * s, nc = c + dc * s;
        while (
          nr >= 0 && nr < size && nc >= 0 && nc < size &&
          b[nr][nc] === player
        ) {
          count++;
          nr += dr * s;
          nc += dc * s;
        }
      }
      if (count >= 5) return true;
    }
    return false;
  }

  /**
   * Place a stone for the current player at (r, c).
   * @param r (number) - row index
   * @param c (number) - column index
   * @returns (boolean) - true if the move was successful, false otherwise
   */
  function place(r: number, c: number): boolean {
    const b = board!;
    if (winner || locked || b[r][c]) return false;
    b[r][c] = current;
    moveNumber++;
    history.push({ row: r, col: c, player: current, move: moveNumber });
    onMove({ row: r, col: c, player: current });
    if (checkWin(r, c, current)) {
      winner = current;
      onWin(current);
    } else if (moveNumber >= size * size) {
      onDraw();
    } else {
      current = current === 'black' ? 'white' : 'black';
    }
    return true;
  }

  /**
   * Make a move for the current player at (r, c).
   * @param r (number) - row index
   * @param c (number) - column index
   * @returns (boolean) - true if the move was successful, false otherwise
   */
  export function move(r: number, c: number): boolean {
    return place(r, c);
  }

  /**
   * Undo the last move made on the board.
   * If there are no moves to undo, the function does nothing.
   */
  export function undo(): void {
    const last = history.pop();
    if (!last) return;
    board![last.row][last.col] = null;
    moveNumber--;
    winner = null;
    current = last.player;
  }

  /**
   * Reset the game to its initial state, clearing the board and history, 
   * and setting the current player to 'black'.
   */
  export function reset(): void {
    board = createBoard();
    history.length = 0;
    moveNumber = 0;
    current = 'black';
    winner = null;
  }
</script>

<div class="gomoku {className}">
  <div
    class="board"
    style="--size:{size}; --cell:32px"
  >
    <div class="grid">
      {#each Array(size - 1) as _, r}
        {#each Array(size - 1) as _, c}
          <div class="tile"></div>
        {/each}
      {/each}
    </div>

    {#each board as row, r}
      {#each row as cell, c}
        <button
          class="cell"
          class:preview={!cell && !winner}
          style="left: calc({c} * var(--cell)); top: calc({r} * var(--cell));"
          onclick={() => place(r, c)}
          disabled={!!winner || !!cell}
          aria-label={`Feld ${r + 1}, ${c + 1}`}
        >
          {#if cell === 'black'}<span class="stone black"></span>{/if}
          {#if cell === 'white'}<span class="stone white"></span>{/if}
          {#if showGhost && !cell && !winner}
            <span class="stone ghost {current}"></span>
          {/if}
        </button>
      {/each}
    {/each}
  </div>
</div>

<style>
  .gomoku {
    display: inline-block;
    font-family: sans-serif;
  }
  .status {
    margin-bottom: 0.5rem;
    display: flex;
    align-items: center;
    gap: 1rem;
  }
  .board {
    position: relative;
    width: calc(var(--cell) * var(--size));
    height: calc(var(--cell) * var(--size));
    background: #d9b06f;
    border: 2px solid #5a3d1a;
    box-sizing: border-box;
  }
  .grid {
    position: absolute;
    left: calc(var(--cell) / 2);
    top: calc(var(--cell) / 2);
    display: grid;
    grid-template-columns: repeat(calc(var(--size) - 1), var(--cell));
    grid-template-rows: repeat(calc(var(--size) - 1), var(--cell));
  }
  .tile {
    border-right: 1px solid #a87f43;
    border-bottom: 1px solid #a87f43;
    border-left: 1px solid #a87f43;
    border-top: 1px solid #a87f43;
  }
  .cell {
    position: absolute;
    width: var(--cell);
    height: var(--cell);
    transform: translate(0, 0);
    margin-left: calc(var(--cell) / 2 - var(--cell) / 2);
    padding: 0;
    background: transparent;
    border: none;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .cell:disabled {
    cursor: default;
  }
  .stone {
    width: 75%;
    height: 75%;
    border-radius: 50%;
  }
  .stone.black {
    background: radial-gradient(circle at 30% 30%, #555, #000);
  }
  .stone.white {
    background: radial-gradient(circle at 30% 30%, #fff, #ccc);
    border: 1px solid #999;
  }
  .stone.ghost {
    opacity: 0;
    transition: opacity 0.1s;
  }
  .cell.preview:hover .stone.ghost {
    opacity: 0.4;
  }
</style>