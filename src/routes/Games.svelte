<script>  
  import UnityGame from "./Games/UnityGame.svelte";
  let game = "Roller"
</script>

<div>
  <h1>{game}</h1>
  <UnityGame game={game} />
</div>

<style>
    div > * {                
        text-align: center;
    }
    div {
        text-align: center;
    }
</style>