<!-- controls for the bingo tile (mark as bingo, unmark, change the text)-->
<script lang="ts">
  export let cardstate: boolean = false;
  export let onToggleBingo: () => void = () => console.log("pooo");
  var bingotext: string = "un-bingo";

  const bbuttonUpdate = function () {
    if (!cardstate) {
      bingotext = "bingo";
    } else {
      bingotext = "un-bingo";
    }
  };

  const toggleBingo = function () {
    cardstate = !cardstate;
    onToggleBingo();
    bbuttonUpdate();
  };

  bbuttonUpdate();
</script>

<div class="tilecontrols">
  <button class="tilecontrol" on:click={(e) => console.log(e)}>Edit</button>
  <button class="tilecontrol bingobutton" on:click={toggleBingo}
    >{bingotext}</button
  >
</div>
