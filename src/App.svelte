<script>
  import { onMount } from "svelte";
  import { getSudokuPair } from "./components/sudoku.svelte";
  let [sudoku, solvedSudoku] = getSudokuPair(1);

  sudoku = sudoku.map(row =>
    row.map(item => {
      if (item === 0) {
        return undefined;
      }
      return item;
    })
  );

  console.log(sudoku);

  onMount(() => {
    document.querySelectorAll("input").forEach(element => {
      if (!element.value) {
        element.readOnly = false;
      } else {
        element.style.fontWeight = "700";
      }
    });
  });

  function arraysMatch(grid1, grid2) {
    for (let row = 0; row < 9; row++) {
      for (let col = 0; col < 9; col++) {
        if (grid1[row][col] !== grid2[row][col]) {
          return false;
        }
      }
    }
    return true;
  }

  $: {
    if (arraysMatch(sudoku, solvedSudoku)) {
      alert("you won!!");
    }
  }
</script>

<style>
  :global(*, body) {
    padding: 0;
  }

  main {
    background-color: deepskyblue;
    min-height: 100vh;
    width: 100vw;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .grid {
    height: 80vh;
    width: 80vh;
    display: grid;
    grid-template-columns: repeat(9, 1fr);
    grid-template-rows: repeat(9, 1fr);
    position: relative;
    border: 3px solid black;
  }

  .grid::after {
    content: "";
    width: 33.3333%;
    border-left: 3px solid black;
    border-right: 3px solid black;
    position: absolute;
    left: 50%;
    top: 0;
    bottom: 0;
    transform: translateX(-50%);
    pointer-events: none;
  }
  .grid::before {
    content: "";
    height: 33.3333%;
    border-top: 3px solid black;
    border-bottom: 3px solid black;
    position: absolute;
    top: 50%;
    left: 0;
    right: 0;
    transform: translateY(-50%);
    pointer-events: none;
  }

  @media (max-aspect-ratio: 1/1) {
    .grid {
      width: 80vw;
      height: 80vw;
    }
  }

  .item {
    justify-content: center;
    align-items: center;
    height: 100%;
    width: 100%;
    position: relative;
    background-color: white;
    transition: 0.2s all;
  }

  input:focus {
    outline: none;
    background-color: #ddd;
  }

  h2 {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
    display: block;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    font-weight: 400;
    font-size: 3vmin;
  }

  input {
    margin: 0;
    padding: 0;
    width: 100%;
    height: 100%;
    border-radius: 0;
    text-align: center;
  }

  input::-webkit-inner-spin-button,
  input::-webkit-outer-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }

  input[type="number"] {
    -moz-appearance: textfield;
  }
</style>

<main>
  <div class="grid">
    {#each sudoku.flat() as item, i}
      <input
        readonly
        type="number"
        bind:value={sudoku[Math.floor(i / 9)][i % 9]} />
    {/each}
  </div>
</main>
