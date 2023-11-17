<!--Board.svelte is a nxm bingos board, made of Tile components -->
<script lang="ts">
  let n: number = 4;
  let m: number = 4;
  let tiles: number[][] = [];
  let count: number = 0;
  let checked: boolean[][] = [];
  let bingo: boolean = false;

  const checkBingo = () => {
    let bingo = true;

    // check rows
    for (let i = 0; i < n; i++) {
      let row = true;
      for (let j = 0; j < m; j++) {
        if (tiles[i][j] == 0) {
          row = false;
          break;
        }
      }
      if (row) {
        return bingo;
      }
    }

    // check columns
    for (let j = 0; j < m; j++) {
      let col = true;
      for (let i = 0; i < n; i++) {
        if (tiles[i][j] == 0) {
          col = false;
          break;
        }
      }
      if (col) {
        return bingo;
      }
    }

    return bingo;
  };

  const checkTile = (i: number, j: number) => {
    if (tiles[i][j] == 0) {
      tiles[i][j] = 1;
      count++;
    } else {
      tiles[i][j] = 0;
      count--;
    }
    checked[i][j] = !checked[i][j];
    bingo = checkBingo();
  };

  const reset = () => {
    tiles = [];
    checked = [];
    count = 0;
    bingo = false;
    for (let i = 0; i < n; i++) {
      tiles.push([]);
      checked.push([]);
      for (let j = 0; j < m; j++) {
        tiles[i].push(0);
        checked[i].push(false);
      }
    }
  };

  reset();

  import Tile from "./Tile.svelte";
</script>

<!-- render board here -->

<div class="board">
  {#each tiles as row, i}
    <div class="row">
      {#each row as tile, j}
        <Tile
          number={i * m + j + 1}
          checked={checked[i][j]}
          onClick={() => checkTile(i, j)}
        />
      {/each}
    </div>
  {/each}
</div>
