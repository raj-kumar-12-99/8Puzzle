class Board {
  constructor(tiles) {
    this.tiles = tiles;
  }

  move(row, col) {
    const tiles = this.tiles.slice();
    const emptyIndex = tiles.indexOf(0);
    const targetIndex = row * 3 + col;

    [tiles[emptyIndex], tiles[targetIndex]] = [tiles[targetIndex], tiles[emptyIndex]];

    return new Board(tiles);
  }

  getTile(row, col) {
    return this.tiles[row * 3 + col];
  }

  toString() {
    let str = '';
    for (let i = 0; i < 9; i += 3) {
      str += `${this.tiles[i]} ${this.tiles[i+1]} ${this.tiles[i+2]}\n`;
    }
    return str;
  }

  isSolved() {
    for (let i = 0; i < 8; i++) {
      if (this.tiles[i] !== i + 1) {
        return false;
      }
    }
    return true;
  }
}

class Solver {
  constructor(initialBoard) {
    this.initialBoard = initialBoard;
  }

  solve() {
    const queue = [];
    queue.push({ board: this.initialBoard, moves: 0 });

    while (queue.length > 0) {
      const { board, moves } = queue.shift();

      if (board.isSolved()) {
        return moves;
      }

      const emptyIndex = board.tiles.indexOf(0);
      const row = Math.floor(emptyIndex / 3);
      const col = emptyIndex % 3;

      if (row > 0) {
        const newBoard = board.move(row - 1, col);
        queue.push({ board: newBoard, moves: moves + 1 });
      }
      if (row < 2) {
        const newBoard = board.move(row + 1, col);
        queue.push({ board: newBoard, moves: moves + 1 });
      }
      if (col > 0) {
        const newBoard = board.move(row, col - 1);
        queue.push({ board: newBoard, moves: moves + 1 });
      }
      if (col < 2) {
        const newBoard = board.move(row, col + 1);
        queue.push({ board: newBoard, moves: moves + 1 });
      }
    }

    return -1; // unsolvable
  }
}

// Example usage:
const initialBoard = new Board([1, 2, 3, 4, 0, 6, 7, 5, 8]);
const solver = new Solver(initialBoard);
const numMoves = solver.solve();
console.log(`Number of moves required to solve: ${numMoves}`);
