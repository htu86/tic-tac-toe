<template>
  <div class="section">
    <h1 class="title">Tic-Tac-Toe</h1>

    <div class="cells-container">
      <div v-for="cell in cells" class="cell">
        <p @click="cellClicked(cell)"> {{ cell.cellValue }} </p>
      </div>
    </div>

    <div class="score-tab">
      <p>X: {{ playerOneScore }}</p>
      <p>O: {{ playerTwoScore }}</p>
    </div>
    <div class="reset-container">
      <button @click="resetGame" class="reset">> Reset Game</button>
    </div>
  </div>
</template>

<script setup>
  import { ref } from 'vue';
  let cells = ref([
    {clicked: false, cellValue: ""},
    {clicked: false, cellValue: ""},
    {clicked: false, cellValue: ""},
    {clicked: false, cellValue: ""},
    {clicked: false, cellValue: ""},
    {clicked: false, cellValue: ""},
    {clicked: false, cellValue: ""},
    {clicked: false, cellValue: ""},
    {clicked: false, cellValue: ""}
  ])

  let winConditions = ref(
    [
      [0, 1, 2],
      [3, 4, 5],
      [6, 7, 8],
      [0, 4, 8],
      [2, 4, 6],
      [0, 3, 6],
      [1, 4, 7],
      [2, 5, 8]
    ]
  )

  let playerOneTurn = ref(true);
  let playerTwoTurn = ref(false);
  let playerOneScore = ref(0);
  let playerTwoScore = ref(0);
  let gameEnded = ref(false);

  function cellClicked(clickedCell){
    if(gameEnded.value == false){
      if(clickedCell.clicked == true){
        console.log("Cell already clicked");
        return;
      }
      swapPlayer(clickedCell, playerOneTurn, playerTwoTurn);
      if(clickedCell.cellValue != ""){
        clickedCell.clicked = true;
      }
      checkWinConditions();
    }
    else{
      return;
    }
  }

  function swapPlayer(clickedCell, playerOneTurn, playerTwoTurn){
    if(playerOneTurn.value == true){
      clickedCell.cellValue = "X";
      playerOneTurn.value = false;
      playerTwoTurn.value = true;
    }
    else if(playerTwoTurn.value == true){
      clickedCell.cellValue = "O";
      playerTwoTurn.value = false;
      playerOneTurn.value = true;
    }
  }

  function checkWinConditions(){
    for(let i = 0; i < winConditions.value.length; i++){
      const firstCell = winConditions.value[i][0];
      const secondCell = winConditions.value[i][1];
      const thirdCell = winConditions.value[i][2];

      if(cells.value[firstCell].cellValue == "X" && cells.value[secondCell].cellValue == "X" && cells.value[thirdCell].cellValue == "X"){
        gameEnded.value = true;
        playerOneScore.value++;
        break;
      }
      if(cells.value[firstCell].cellValue == "O" && cells.value[secondCell].cellValue == "O" && cells.value[thirdCell].cellValue == "O"){
        gameEnded.value = true;
        playerTwoScore.value++;
        break;
      }
    }
    for(let i = 0; i < cells.value.length; i++){
      if(cells.value[i].cellValue != ""){
        if(i == cells.value.length - 1 && gameEnded.value == false){
          console.log("draw");
        }
      }
      else{
        break;
      }
    }
  }

  function resetGame(){
    for(let i = 0; i < cells.value.length; i++){
      cells.value[i].cellValue = "";
      cells.value[i].clicked = false;
    }
    gameEnded.value = false;
  }


</script>

<style>
  .section {
    height: 100vh;
    width: 100vw;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    flex-shrink: 0;
  }

  .title {
    margin-bottom: 3rem;
    font-size: 5em;
  }

  .cells-container { 
    display: grid;
    justify-content: center;
    align-content: center;
    grid-template-columns: auto auto auto;
    gap:1rem;
  }

  .cell {
    background-color: #fff;
    width: 120px;
    height: 120px;
    border-radius: 15px;
  }

  .cell > p {
    padding: 1.2rem;
    font-size: 4rem;
    font-weight: 300;
    height: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    user-select: none;
    background: linear-gradient(62deg, #FBAB7E 0%, #F7CE68 100%);
    background-clip:text ;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent
  }

  .reset-container {
    width: 12rem;
    height: 3rem;
    background-color:#fff;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 15px;
    transition: all 0.4s ease;
  }

  .reset {
    width: 12rem;
    height: 3rem;
    font-size: 1.4em;
    border: none;
    color: transparent;
    background-image: linear-gradient(62deg, #FBAB7E 0%, #F7CE68 100%);
    -webkit-background-clip: text;
    background-clip: text;
    cursor: pointer;
  }
  
  .score-tab {
    display: flex;
    flex-direction: row;
    gap:5rem;
    margin: 2rem;
    font-size: 2em;
  }

  /* Responsivity */

  @media screen and (max-width: 520px) {
    .title{
      font-size: 3.2em;
    }
    .cell{
      width: 100px;
      height: 100px;
    }
    .score-tab{
      font-size: 1.8em;
    }
  }

</style>