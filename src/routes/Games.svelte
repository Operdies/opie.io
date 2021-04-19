<script>
  import UnityGame from "./Games/UnityGame.svelte";
  let Game = "Roller"  
</script>

<div>
  <h1>{Game}</h1>
  <UnityGame Game={Game} />
</div>

<style>
    div > * {                
        text-align: center;
    }
    div {
        text-align: center;
    }
</style>