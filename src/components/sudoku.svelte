<script context="module">
  function makeEmptyGrid() {
    let grid = [];
    for (let i = 0; i < 9; i++) {
      let row = [];
      for (let j = 0; j < 9; j++) {
        row = [...row, 0];
      }
      grid = [...grid, row];
    }
    return grid;
  }

  function printGrid(grid) {
    console.log("");
    grid.forEach(row => {
      console.log(row.join(" "));
    });
    console.log("");
  }

  function isFilled(grid) {
    if (grid.flat().includes(0)) {
      return false;
    }
    return true;
  }

  function isValid(val, row, col, grid) {
    const colValues = grid.map(row => row[col]).flat();
    if (!grid[row].includes(val) && !colValues.includes(val)) {
      let square = [];
      if (col < 3) {
        if (row < 3) {
          square = grid.slice(0, 3).map(row => row.slice(0, 3));
        } else if (row < 6) {
          square = grid.slice(3, 6).map(row => row.slice(0, 3));
        } else {
          square = grid.slice(6, 9).map(row => row.slice(0, 3));
        }
      } else if (col < 6) {
        if (row < 3) {
          square = grid.slice(0, 3).map(row => row.slice(3, 6));
        } else if (row < 6) {
          square = grid.slice(3, 6).map(row => row.slice(3, 6));
        } else {
          square = grid.slice(6, 9).map(row => row.slice(3, 6));
        }
      } else {
        if (row < 3) {
          square = grid.slice(0, 3).map(row => row.slice(6, 9));
        } else if (row < 6) {
          square = grid.slice(3, 6).map(row => row.slice(6, 9));
        } else {
          square = grid.slice(6, 9).map(row => row.slice(6, 9));
        }
      }
      if (!square.flat().includes(val)) {
        return true;
      }
      return false;
    }
  }

  let counter = 0;
  function solveGrid(grid) {
    let row;
    let col;
    for (let i = 0; i < 81; i++) {
      row = Math.floor(i / 9);
      col = i % 9;
      if (grid[row][col] === 0) {
        for (var val of [...Array(10).keys()].slice(1)) {
          if (isValid(val, row, col, grid)) {
            grid[row][col] = val;
            if (isFilled(grid)) {
              counter++;
              break;
            } else {
              if (solveGrid(grid)) {
                return true;
              }
            }
          }
        }
        val = 1;
        break;
      }
    }
    grid[row][col] = 0;
  }

  // From Internet
  function shuffle(array) {
    var currentIndex = array.length,
      temporaryValue,
      randomIndex;

    while (0 !== currentIndex) {
      randomIndex = Math.floor(Math.random() * currentIndex);
      currentIndex -= 1;

      temporaryValue = array[currentIndex];
      array[currentIndex] = array[randomIndex];
      array[randomIndex] = temporaryValue;
    }

    return array;
  }

  function fillGrid(grid) {
    let row;
    let col;
    for (let i = 0; i < 81; i++) {
      row = Math.floor(i / 9);
      col = i % 9;
      if (grid[row][col] === 0) {
        for (var val of shuffle([...Array(10).keys()].slice(1))) {
          if (isValid(val, row, col, grid)) {
            grid[row][col] = val;
            if (isFilled(grid)) {
              return true;
            } else {
              if (fillGrid(grid)) {
                return true;
              }
            }
          }
        }
        val = 1;
        break;
      }
    }
    grid[row][col] = 0;
  }

  function removeNumbers(grid, n) {
    let attempts = n;
    let row;
    let col;
    while (attempts > 0) {
      do {
        row = Math.floor(Math.random() * 9);
        col = Math.floor(Math.random() * 9);
      } while (grid[row][col] === 0);
      const backup = grid[row][col];
      grid[row][col] = 0;
      const copyGrid = JSON.parse(JSON.stringify(grid));
      counter = 0;
      solveGrid(copyGrid);
      if (counter > 1) {
        grid[row][col] = backup;
        attempts--;
      }
    }
  }

  export function getSudokuPair(n) {
    const grid = makeEmptyGrid();
    fillGrid(grid);
    const gridSolved = JSON.parse(JSON.stringify(grid));
    removeNumbers(grid, n);
    return [grid, gridSolved];
  }
</script>
