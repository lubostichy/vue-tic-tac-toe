<script setup>
import { ref, computed, reactive } from 'vue';
import Board from './Board.vue';
const history = reactive([{
    squares: Array(9).fill(null),
    coordinationX: null,
    coordinationY: null
}]);
const xIsNext = ref(true);
const stepNumber = ref(0);
const current = history[stepNumber.value];
const status = computed(() => {
    const winner = calculateWinner(current.squares);
    return winner ? 
        "Winner: " + winner :
        "Next player: " + (xIsNext.value ? 'X':'O')
});
const moves = computed(() => {
    return history.map((step, move) => {
        const desc = move ? 
        "Move #" + move + " (" + step.coordinationX + "," + step.coordinationY + ")" :
        "Start"
        return desc;
    });
});

function handleClick(id) {
    if (calculateWinner(current.squares) || current.squares[id]) {
        return;
    }
    current.squares[id] = xIsNext.value ? 'X' : 'O';
    xIsNext.value = !xIsNext.value;
    history.push({
        squares: current.squares,
        coordinationX: calculateColumn(id),
        coordinationY: calculateRow(id)
    });
    stepNumber.value = history.length;
}
function calculateWinner(squares) {
    const lines = [
      [0, 1, 2],
      [3, 4, 5],
      [6, 7, 8],
      [0, 3, 6],
      [1, 4, 7],
      [2, 5, 8],
      [0, 4, 8],
      [2, 4, 6],
    ];
    for (let i = 0; i < lines.length; i++) {
      const [a, b, c] = lines[i];
      if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
        return squares[a];
      }
    }
    return null;
}

function calculateColumn(index) {
  switch (index) {
    case 0:
    case 3:
    case 6:
      return 1;
    case 1:
    case 4:
    case 7:
      return 2;
    case 2:
    case 5:
    case 8:
      return 3;
    default:
      return '';
  }
}

function calculateRow(index) {
  switch (index) {
    case 0:
    case 1:
    case 2:
      return 1;
    case 3:
    case 4:
    case 5:
      return 2;
    case 6:
    case 7:
    case 8:
      return 3;
    default:
      return '';
  }
}

</script>
<template>
    <div className="game">
        <div className="game-board">
            <Board :squares="current.squares" @handleClick="handleClick"/>
        </div>
    </div>
    <div className="game-info">
        <div>{{ status }}</div>
        <ol>
            <li v-for="move in moves">
                {{ move }}
            </li>
        </ol>
    </div>
</template>