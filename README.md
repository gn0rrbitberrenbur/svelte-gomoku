# svelte-gomoku: Playable gomoku component for Svelte
Playable gomoku component for Svelte.

## Features
- Track game states via props
- Fully restylable via css
- Move history
- Typed

## Usage
### Installation
```
npm install https://github.com/gn0rrbitberrenbur/svelte-gomoku#main
```

### Basic usage
Basic playable board:
```
<script>
  import { Gomoku } from '$lib';
</script>

<Gomoku />
```

## Props
The game can be observed by binding to props.

| Prop | Type | Bindable | Access | Default |
|------|-----|----------|---------|---------|
| size | `number` | ✓ | read/write | `15` |
| showGhost | `boolean` | ✓ | read/write | `true` |
| moveNumber | `number` | ✓ | read/write | `0` |
| history | `Move[]` | ✓ | read/write | `[]` |
| board | `(Player \| null)[][]` | ✓ | read/write | `undefined` |
| current | `Player` | ✓ | read/write | `'black'` |
| winner | `Player \| null` | ✓ | read/write | `null` |
| locked | `boolean` | ✓ | read/write | `false` |
| onWin | `(player: Player) => void` | ✕ | write (Callback) | `() => {}` |
| onMove | `(m: { row: number; col: number; player: Player }) => void` | ✕ | write (Callback) | `() => {}` |
| onDraw | `() => void` | ✕ | write (Callback) | `() => {}` |

## Methods
Export functions available via component reference:

| Method | Parameters | Description |
|--------|-----------|-------------|
| move | `(r: number, c: number): boolean` | Place a stone at (r, c) |
| undo | `(): void` | Undo the last move |
| reset | `(): void` | Reset the game to initial state |

## Themes
Available themes: `theme-blue`, `theme-green`, `theme-pink`

```
<Gomoku class="theme-blue" />
```

Create custom themes by using the template in [`static/style-template.css`](static\style-template.css)

## Types
### `Player`
The `Player` type describes whether a player is playing the black or the white stones.<br>
Properties:
- `'black' | 'white'`: The colour of the players stones

### `Move`
The `Move` type describes a gomoku move.<br>
Properties:
- `row`: (number), the row index of the move
- `col`: (number), the column index of the move
- `player`: (Player), the player who made the move
- `move`: (number), the sequential number of the move