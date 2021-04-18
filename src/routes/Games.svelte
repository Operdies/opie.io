<script>
  import TicTacToe from "./Games/TicTacToe.svelte";
  import UnityGame from "./Games/UnityGame.svelte";
</script>

<div>
  <h1>Roll A Ball</h1>
  <UnityGame game={"Roller"} />
  <h1>Tic Tac Toe</h1>
  <TicTacToe />
</div>

<style>
    div > * {
        text-align: center;
        margin-top: 10%;
    }
</style>