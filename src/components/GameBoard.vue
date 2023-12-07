<template>
  <div class="root">
    <div class="info">
      <div class="currentPlayer">{{ currentPlayer }}'s move</div>
      <div class="stata currentPlayer">
        <p>x won: {{ xWon }}</p>
        <p>o won: {{ oWon }}</p>
      </div>
      <div class="restart"><button @click="restart()">Restart</button></div>
    </div>
    <div class="container">
      <div class="title">tic-tac-toe</div>
      <div class="game-board">
        <div v-for="(row, rowIndex) in board" :key="rowIndex" class="row">
          <div v-for="(cell, colIndex) in row" :key="colIndex" class="cell" @click = "makeMove(rowIndex, colIndex)">
            {{ cell }}
          </div>
        </div>
      </div>
    </div>
  </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        board: [
          ['', '', ''],
          ['', '', ''],
          ['', '', '']
        ],
        currentPlayer: 'x',
        winChar: '',
        xWon: 0,
        oWon: 0,
        isFinish: false
      };
    },
    methods: {
      makeMove(row, col) {
        if (!this.isFinish) {
          if (this.board[row][col] === '') {
            if (this.currentPlayer === 'x') {
              this.$set(this.board[row], col, 'X');
              this.currentPlayer = 'o';
            } else {
              this.$set(this.board[row], col, 'O');
              this.currentPlayer = 'x';
            }
            this.checkWin()
          }
        }
        let count = 0;
        for(let i = 0; i < 3; i++){
          for(let j = 0; j < 3; j++){
            if(this.board[i][j] !== ''){
              count++;
            }
          }
        }
        if(count === 9) {
          this.isFinish = true
          this.gameTie();
        }
      },
      checkWin(){
        let win = false
        for(let i = 0; i < 3; i++) {
          let char = this.board[i][0]
          if (char === '') continue

          for (let j = 0; j < 3; j++) {
            if (this.board[i][j] === char) {
              if (j === 2) {
                win = true
                this.winChar = char
              }

            } else {
              break
            }
          }
        }

        for(let i = 0; i < 3; i++){
          let char = this.board[0][i]
          if(char === '') continue

          for(let j = 0; j < 3; j++) {
            if(this.board[j][i] === char) {
              if(j === 2) {
                win = true
                this.winChar = char
              }

            }else{
              break
            }
          }
        }

        this.playerWin(win, this.winChar)

        if(this.board[1][1] !== '') {

          if ((this.board[1][1] === this.board[2][2])) {
            if (this.board[1][1] === this.board[0][0]) {
              this.playerWin(true, this.board[1][1])
            }

          } else if ((this.board[1][1] === this.board[2][0])) {
            if (this.board[1][1] === this.board[0][2]) {
              this.playerWin(true, this.board[1][1])
            }
          }
        }
      },
      playerWin(ifTrue, winChar){
        if(ifTrue === true){
          setTimeout(() => {
            alert(`Player ${winChar} wins!!!`)
            if(winChar === 'X') this.xWon++
            else this.oWon++

            this.isFinish = true
          }, 100)
        }
      },
      restart(){
        for(let i = 0; i < 3; i++)
          for(let j = 0; j < 3; j++)
          this.$set(this.board[i], j, '');
      this.isFinish = false
      this.currentPlayer = 'x'
      },
      gameTie(){
        setTimeout(() => {
          alert('Tie!')
        }, 100  )
      }
    }
  };
  </script>
  
  <style scoped>
  /* Add styling for the game board and cells */
  @import url('https://fonts.googleapis.com/css2?family=Pixelify+Sans:wght@400;500&family=Poppins&display=swap');
  *{
    font-family: 'Pixelify Sans', sans-serif;
  }
  .root{
    display: flex;
    justify-content: center;
    margin-top: 60px;
  }
  .container{
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .currentPlayer{
    font-size: 30px;
    height: fit-content;
    min-width: 180px;
    background-color: #d3d6db;
    border-radius: 1px;
    text-align: center;
    padding: 20px;
    margin-right: 50px;
    margin-top: 160px;
  }
  .stata{
    margin-top: 5px;
    padding:  20px;
    font-size: 24px;
    height: fit-content;
  }
  button{
    margin: 20px;
    width: 100px;
    height: 40px;
  }

  .title{
    font-size: 50px;
  }

  .game-board {
    display: flex;
    flex-direction: column;
    gap: 10px;
  }
  
  .row {
    display: flex;
    flex-direction: row;
    gap: 10px;
  }
  
  .cell {
    width: 150px;
    height: 150px;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #f0f0f0;
    font-size: 50px;
  }

  .winner{
    color: red;
  }
  </style>