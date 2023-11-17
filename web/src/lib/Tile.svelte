<!-- a tile is a bingo tile -->
<script lang="ts">
  export let checked: boolean = false;
  export let number: number = 0;
  export let onClick: () => void;

  let details: boolean = false;
  let debouncedDetails: boolean = false;

  function debounce(fn: () => void, delay: number) {
    let timer: number;
    return function () {
      clearTimeout(timer);
      timer = setTimeout(() => {
        fn();
      }, delay);
    };
  }

  const showDetails = () => {
    details = true;
    debounce(() => {
      debouncedDetails = details;
      console.log(details, debouncedDetails);
    }, 800)();
  };

  const hideDetails = () => {
    details = false;
    debouncedDetails = false;
  };

  import TileControls from "./TileControls.svelte";
</script>

<div
  class="tile"
  class:tile-bingo={checked}
  class:tile-checked={checked}
  on:mouseenter={showDetails}
  on:mouseleave={hideDetails}
>
  Proto fuckup
  {#if debouncedDetails}
    <TileControls cardstate={checked} onToggleBingo={onClick} />
  {/if}
</div>
